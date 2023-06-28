# Querying for Elements With Different Criteria

React Testing Library provides many different query functions.  Each begins with a name like `getBy`, `findBy`, etc.  The names also have common endings.  The different name endings indicate how the query for an element will be performed.

| End of Function Name | Search Criteria                                                    |
|----------------------|--------------------------------------------------------------------|
| ByRole               | Finds elements based on their implicit or explicit ARIA role       |
| ByLabelText          | Find form elements based upon the text their paired labels contain |
| ByPlaceholderText    | Find form elements based upon their placeholder text               |
| ByText               | Find elements based upon the text they contain                     |
| ByDisplayValue       | Find elements based upon their current value                       |
| ByAltText            | Find elements based upon their `alt` attribute                     |
| ByTitle              | Find elements based upon their `title` attribute                   |
| ByTestId             | Find elements based upon their `data-testid` attribute             |

## When to Use Each

Always prefer using query functions ending with `ByRole`.  Only use others if `ByRole` is not an option.

```javascript
import { screen, render } from '@testing-library/react';
import { useState } from 'react';

function DataForm() {
  const [email, setEmail] = useState('asdf@asdf.com');

  return (
    <form>
      <h3>Enter Data</h3>

      <div data-testid="image wrapper">
        <img alt="data" src="data.jpg" />
      </div>

      <label htmlFor="email">Email</label>
      <input 
        id="email"
        value={email}
        onChange={e => setEmail(e.target.value)}
      />

      <label htmlFor="color">Color</label>
      <input id="color" placeholder="Red" />

      <button title="Click when ready to submit">
        Submit
      </button>
    </form>
  );
}

render(<DataForm />);
```

```javascript
test('selecting different elements', () => {
  render(<DataForm />);

  const elements = [
    screen.getByRole('button'),
    screen.getByText(/enter/i),

    screen.getByLabelText(/email/i),
    screen.getByPlaceholderText('Red'),
    screen.getByDisplayValue('asdf@asdf.com'),
    screen.getByAltText('data'),
    screen.getByTitle(/ready to submit/i),

    screen.getByTestId('image wrapper')
  ];

  for (let element of elements) {
    expect(element).toBeInTheDocument();
  }
});
```