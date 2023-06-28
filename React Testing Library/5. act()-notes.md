# Errors

The component we are testing: 
```javascript
import { Link } from 'react-router-dom';
import FileIcon from '../tree/FileIcon';
import RepositoriesSummary from './RepositoriesSummary';

function RepositoriesListItem({ repository }) {
  const { full_name, language, description, owner, name, svn_url } = repository;

  return (
    <div className="py-3 border-b flex">
      <FileIcon name={language} className="shrink w-6 pt-1" />
      <div>
        <Link to={`/repositories/${full_name}`} className="text-xl">
          {owner.login}/<span className="font-bold">{name}</span>
        </Link>
        <p className="text-gray-500 italic py-1">{description}</p>
        <RepositoriesSummary repository={repository} />
      </div>
      <a href={svn_url}>repo</a>
    </div>
  );
}

export default RepositoriesListItem;
```

## Error with the React Router:
When working on React applications we use outside libraries. As we're writing tests, these libraries are not always happy about being executed in a test environment.

This test example fails, because of the `Link` component.
The `Link` component is only going to work correctly if it has a React router context available to it.
So in our test environment, when the link is rendered, it tries to reach up and find that context.
But the context doesn't exist because we didn't create it.
And so the link ends up throwing an error.

```javascript
import { render, screen } from '@testing-library/react'
import RepositoriesListItem from './RepositoriesListItem'

function renderComponent() {
    const repository = {
        full_name: 'Project',
        language: 'JavaScript',
        description: 'Very good',
        owner: 'Meta',
        name: 'Project',
        svn_url: 'https://github.com/facebook/react'
    }
    render(<RepositoriesListItem repository={repository} />)

}

test('shows a link', () => {
    renderComponent()
})
```

To fix this we can try to wrap our component that we're trying to test in a `BrowserRouter`, a `HashRouter` or a `MemoryRouter`.
`BrowserRouter` - Stores current URL in the address bar.
`HashRouter`  - Stores  current URLL in the #part of the address bar. 
`MemoryRouter` - Stores current URL in memory.

In this example we will use the `MemoryRouter`.

```javascript
import { render, screen } from '@testing-library/react';
import { MemoryRouter } from 'react-router-dom';
import RepositoriesListItem from './RepositoriesListItem';

function renderComponent() {
  const repository = {
    full_name: 'facebook/react',
    language: 'Javascript',
    description: 'A js library',
    owner: 'facebook',
    name: 'react',
    html_url: 'https://github.com/facebook/react',
  };
  render(
    <MemoryRouter>
      <RepositoriesListItem repository={repository} />
    </MemoryRouter>
  );
}

test('shows a link to the github homepage for this repository', () => {
  renderComponent();
});
```

## The `act()` warning 

```shell
Warning: An update to FileIcon inside a test was not wrapped in act(...).
When testing, code that causes React state updates should be wrapped into act(...):
  act(() => {
    /* fire events that update state */
  });
    /* assert on the output */
```

This warning will occur if you're doing data fetching in `useEffect()`.

The component that is causing the warning: 

```javascript
import '@exuanbo/file-icons-js/dist/css/file-icons.css';
import icons from '@exuanbo/file-icons-js/dist/js/file-icons';
import { useEffect, useState } from 'react';
import classNames from 'classnames';

function FileIcon({ name, className }) {
  const [klass, setKlass] = useState('');

  useEffect(() => {
    icons
      .getClass(name)
      .then((k) => setKlass(k))
      .catch(() => null);
  }, [name]);

  if (!klass) {
    return null;
  }

  return (
    <i
      role="img"
      aria-label={name}
      className={classNames(className, klass)}
    ></i>
  );
}

export default FileIcon;
```

In short:
1. Unexpected state updates in tests are bad. 
2. The act function defines a window in time where state updates can (and should) occur.
3. RTL uses `act` behind the scenes.
4. To solve act warnings, you should use a `findBy`. Usually you don't want to follow the advice of the warning. 

### Why unexpected state updates in a testing environment are bad?

The component:
```javascript
function UsersList() {
  const [shouldLoad, setShouldLoad] = useState( false); 
  const [users, setUsers] = useState([]);
  useEffect(() => {
  if (shouldLoad) {
    // promise! Async code!
    fetchUsers().then(data => setUsers(data) )
    }
  }, [shouldLoad]);
  return (
    <button onClick={() => setShouldLoad(true)}>
      Load 
    </button>
  );
}
```
The test:
```javascript
test('clicking the button loads users', () => {
  render(<UsersList />);
  const button = screen.getByRole('button');
  user.click(button);
  const users = screen.getAllByRole('listitem');
  expect(users).toHaveLength(3);
);
```
The problem with this test is that after we click on that button, we're then going to immediately go down to the next line of code and try to find all the different users that presumably we have fetched and rendered out on the screen. And at that point in time, our data fetching request is still pending. There's no list and the **test will fail**.

### act() and RTL

The act function, this is a function that is implemented by React Dom. It defines a window in time where a state update can and should occur inside of one of our tests.

Let's write the same test without RTL with the `act()` function.
```javascript
import { render } from 'react-dom';
import { act } from 'react-dom/test-utils'

test('clicking the button loads users', () => {
  
  act (() => {
  render (<UsersList />, container);
  });
  
  const button = document.querySelector('button');
  await act(async () => {
    button.dispatch(new MouseEvent('click'))
  });
  
  const users = document.querySelectorAll('li');
  expect(users).toHaveLength(3);
});
```

Whenever we are not using RTL, if we run some code that's going to change our state, it must be wrapped in an act function call. The act function, gives us a little window in time. We can update our state in there. 

RTL uses the act function for you behind the scenes.

These functions automatically call `act()` for you.
- `screen.findBy()` (is asynchronous)
- `screen.findAllBy()` (is asynchronous)
- `waitFor` (is asynchronous)
- `user.keyboard` (is synchronous)
- `user.click`  (is synchronous)

So whenever we are using React Testing library, this is the preferred way of using `act()`. We do not use the `act` function directly.



## The correct ways to fix the warning:

### Using the query
```javascript
import { render, screen } from '@testing-library/react';
import { MemoryRouter } from 'react-router-dom';
import RepositoriesListItem from './RepositoriesListItem';

function renderComponent() {
  const repository = {
    full_name: 'facebook/react',
    language: 'Javascript',
    description: 'A js library',
    owner: 'facebook',
    name: 'react',
    html_url: 'https://github.com/facebook/react',
  };
  render(
    <MemoryRouter>
      <RepositoriesListItem repository={repository} />
    </MemoryRouter>
  );
}

test('shows a link to the github homepage for this repository', async () => {
  renderComponent();

  await screen.findByRole('img', { name: 'Javascript' });
});
```

### Using a module mock to avoid rendering the troublesome component

```javascript
import { render, screen } from '@testing-library/react';
import { MemoryRouter } from 'react-router-dom';
import RepositoriesListItem from './RepositoriesListItem';

jest.mock('../tree/FileIcon', () => {
  // Content of FileIcon.js
  // You no longer have the real file 
  // You'll get a fake component
  return () => {
    return 'File Icon Component';
  };
});

function renderComponent() {
  const repository = {
    full_name: 'facebook/react',
    language: 'Javascript',
    description: 'A js library',
    owner: 'facebook',
    name: 'react',
    html_url: 'https://github.com/facebook/react',
  };
  render(
    <MemoryRouter>
      <RepositoriesListItem repository={repository} />
    </MemoryRouter>
  );
}

test('shows a link to the github homepage for this repository', async () => {
  renderComponent();

  screen.debug();
});
```


## The final code for the test

The component: 
```javascript
import { Link } from 'react-router-dom';
import { MarkGithubIcon } from '@primer/octicons-react';
import FileIcon from '../tree/FileIcon';
import RepositoriesSummary from './RepositoriesSummary';

function RepositoriesListItem({ repository }) {
  const { full_name, language, description, owner, name } = repository;

  return (
    <div className="py-3 border-b flex">
      <FileIcon name={language} className="shrink w-6 pt-1" />
      <div>
        <Link to={`/repositories/${full_name}`} className="text-xl">
          {owner.login}/<span className="font-bold">{name}</span>
        </Link>
        <p className="text-gray-500 italic py-1">{description}</p>
        <RepositoriesSummary repository={repository} />
      </div>
      <div className="grow flex items-center justify-end pr-2">
        <a href={repository.html_url} aria-label="github repository">
          <MarkGithubIcon />
        </a>
      </div>
    </div>
  );
}

export default RepositoriesListItem;

```

The test:
```javascript
import { render, screen } from '@testing-library/react';
import { MemoryRouter } from 'react-router-dom';
import RepositoriesListItem from './RepositoriesListItem';

function renderComponent() {
  const repository = {
    full_name: 'facebook/react',
    language: 'Javascript',
    description: 'A js library',
    owner: 'facebook',
    name: 'react',
    html_url: 'https://github.com/facebook/react',
  };
  render(
    <MemoryRouter>
      <RepositoriesListItem repository={repository} />
    </MemoryRouter>
  );

  return { repository };
}

test('shows a link to the github homepage for this repository', async () => {
  const { repository } = renderComponent();

  await screen.findByRole('img', { name: 'Javascript' });

  const link = screen.getByRole('link', {
    name: /github repository/i,
  });
  expect(link).toHaveAttribute('href', repository.html_url);
});

```

Additional tests:
```javascript
test('shows a fileicon with the appropriate icon', async () => {
  renderComponent();

  const icon = await screen.findByRole('img', { name: 'Javascript' });

  expect(icon).toHaveClass('js-icon');
});
```

```javascript
test('shows a link to the code editor page', async () => {
  const { repository } = renderComponent();

  await screen.findByRole('img', { name: 'Javascript' });

  const link = await screen.findByRole('link', {
    name: new RegExp(repository.owner.login),
  });

  expect(link).toHaveAttribute('href', `/repositories/${repository.full_name}`);
});
```