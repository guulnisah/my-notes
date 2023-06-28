# Data fetching in Tests

- We don't want our components to make actual network  requests. 
- Slow! Data might change. 
- We fake (or mock) data fetchin in tests.

## Options for data fetching
1. Mock the file that contains the data fetchin code. 
2. Use a library to 'mock' axios - get axios to return fake data.
3. Create a manual mock for axios. 

### Module Mocking

The code of the hook: 
```javascript
import axios from 'axios';
import useSWR from 'swr';

async function repositoriesFetcher([url, searchQuery]) {
  const res = await axios.get(url, {
    params: {
      q: searchQuery || '',
    },
  });

  return res.data.items;
}

export default function useRepositories(searchQuery) {
  const { data, error, isLoading } = useSWR(
    searchQuery && ['/api/repositories', searchQuery],
    repositoriesFetcher
  );

  return {
    data,
    isLoading,
    error,
  };
}
```
The hook fetches data. 
Instead of getting data from the hook, we import fake data.

```javascript
import { screen, render } from '@testing-library/ react' 
import HomeRoute from ' . /HomeRoute' ;

jest.mock('./hooks/useRepositories', () => {
    return () => {
        return {
            data: [ 
                { name: 'react' },
                { name: 'bootstrap' }, 
                { name: 'javascript' }
            ]
        }
    }
});
```

The problem with this approach: now the interaction between our component and the hook is completely untested. The hook may be used incorrectly, but we'll never know. 

### Using a library

When we make a network request to an API to get some data. We don't want this inside a test. 
We can use aa library called MSW (mock service worker).
We're still going to use Axios or Fetch, but rather than sending that out to some outside API, the library is going to intercept the request.
So no request actually leaves our test environment.

#### MSW Setup

1. Create a test file
2. Understand the exact URL, method, and return value of requests that your component will make
3. Create a MSW handler to intercept that request and return some fake data for your component to use
4. Set up the beforeAll, afterEach, and afterAll hooks in your test file
5. In a test, render the component. Wait for an element to be visible

```javascript
import { render, screen } from '@testing-library/react';
import { setupServer } from 'msw/node';
import { rest } from 'msw';
import { MemoryRouter } from 'react-router-dom';
import HomeRoute from './HomeRoute';

const handlers = [
  rest.get('/api/repositories', (req, res, ctx) => {
    const query = req.url.searchParams.get('q');
    console.log(query);

    return res(
      ctx.json({
        items: [
          { id: 1, full_name: 'full name!!!' },
          { id: 2, full_name: 'other name!!!' },
        ],
      })
    );
  }),
];
const server = setupServer(...handlers);

beforeAll(() => {
  server.listen();
});
afterEach(() => {
  server.resetHandlers();
});
afterAll(() => {
  server.close();
});


test('renders two links for each language', async () => {
  render(
    <MemoryRouter>
      <HomeRoute />
    </MemoryRouter>
  );

  // Loop over each language
  const languages = [
    'javascript',
    'typescript',
    'rust',
    'go',
    'python',
    'java',
  ];
  for (let language of languages) {
    // For each language, make sure we see two links
    const links = await screen.findAllByRole('link', {
      name: new RegExp(`${language}_`),
    });

    expect(links).toHaveLength(2);
    expect(links[0]).toHaveTextContent(`${language}_one`);
    expect(links[1]).toHaveTextContent(`${language}_two`);
    expect(links[0]).toHaveAttribute('href', `/repositories/${language}_one`);
    expect(links[1]).toHaveAttribute('href', `/repositories/${language}_two`);
  }
});

```

Your fake handler should be written in a way that makes it reusable.

```javascript
import { setupServer } from 'msw/node';
import { rest } from 'msw';

export function createServer(handlerConfig) {
  const handlers = handlerConfig.map((config) => {
    return rest[config.method || 'get'](config.path, (req, res, ctx) => {
      return res(ctx.json(config.res(req, res, ctx)));
    });
  });
  const server = setupServer(...handlers);

  beforeAll(() => {
    server.listen();
  });
  afterEach(() => {
    server.resetHandlers();
  });
  afterAll(() => {
    server.close();
  });
}

```

```javascript
import { render, screen } from '@testing-library/react';
import { setupServer } from 'msw/node';
import { rest } from 'msw';
import { MemoryRouter } from 'react-router-dom';
import HomeRoute from './HomeRoute';
import { createServer } from '../test/server';

createServer([
  {
    path: '/api/repositories',
    res: (req) => {
      const language = req.url.searchParams.get('q').split('language:')[1];
      return {
        items: [
          { id: 1, full_name: `${language}_one` },
          { id: 2, full_name: `${language}_two` },
        ],
      };
    },
  },
]);

test('renders two links for each language', async () => {
  render(
    <MemoryRouter>
      <HomeRoute />
    </MemoryRouter>
  );

  // Loop over each language
  const languages = [
    'javascript',
    'typescript',
    'rust',
    'go',
    'python',
    'java',
  ];
  for (let language of languages) {
    // For each language, make sure we see two links
    const links = await screen.findAllByRole('link', {
      name: new RegExp(`${language}_`),
    });

    expect(links).toHaveLength(2);
    expect(links[0]).toHaveTextContent(`${language}_one`);
    expect(links[1]).toHaveTextContent(`${language}_two`);
    expect(links[0]).toHaveAttribute('href', `/repositories/${language}_one`);
    expect(links[1]).toHaveAttribute('href', `/repositories/${language}_two`);
  }
});

```