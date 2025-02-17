# ⚠️ Things to AVOID When Building React Components 

## Cofount Labs - Mandatory React Coding Practices

**For Pull Request Approval and Becoming a Killer Developer**

At Cofount Labs, adhering to these React coding practices is **not optional**.  These are **mandatory standards** you **MUST** follow for:

* **Pull Request Approval:** Your code will be rigorously reviewed against these guidelines.
* **Becoming a Killer Developer:**  Mastering these practices is crucial for your growth and recognition as a top-tier React developer at Cofount Labs.
* **Preventing Future Refactoring:**  These standards are designed to ensure code maintainability and scalability from the outset, significantly reducing the need for time-consuming and costly refactoring down the line.
* **Saving Project Time:** By writing clean, consistent, and efficient code, we minimize debugging, improve collaboration, and accelerate project delivery across all projects.

**Failure to comply with these standards will result in pull request rejection and hinder your progress as a developer at Cofount Labs.**

---

**1. Think "Component Library"!**

   Imagine you're building a set of LEGO bricks (components). Each brick should be useful on its own and able to connect to other bricks in many ways.

   **Instead:**  Design your components so they can be reused in lots of different places in your app.  This saves you time and makes your app code cleaner!

**2. Don't Hardcode Colors!**

   Imagine wanting to change the main color of your app from blue to green. If you've written `color: 'blue'` everywhere, you're in for a lot of work!

   **Instead:** Think of colors like variables! Use CSS variables (like `--primary-color: blue;`) or theming (we'll learn about this later!) so you can change colors in one place and it updates everywhere.

**3. Don't Hardcode Text in Your Components!**

   Let's say you have a button that always says "Click Me". What if you need another button that says "Submit"?  If "Click Me" is *inside* the button's code, you have to make a whole new button component!

   **Instead:** Use **props**! Props are like instructions you give to your component.  You can make a `Button` component and use a `text` prop:  `<Button text="Click Me" />` and `<Button text="Submit" />`. Now, one button component, many texts! 🎉

**4. Data Fetching in Components? Maybe Not Directly!**

#### Data Fetching with Custom Hooks in React

This project demonstrates how to separate data fetching logic from React components using custom hooks, specifically by placing hooks in a dedicated `hooks` folder.

#### Why Separate Data Fetching?

For larger React applications, embedding data fetching directly within components can lead to:

* **Messy and cluttered components:** Components become harder to read and understand when they handle both UI rendering and data retrieval.
* **Code duplication:**  Fetching similar data in different components can lead to repetitive code.
* **Difficult testing:** Testing components becomes more complex when data fetching is tightly coupled within them.

#### Solution: Custom Hooks and the `hooks` Folder

We utilize React's **custom hooks** to encapsulate data fetching logic. These hooks are placed in a `hooks` folder to keep them organized and separate from components.

**Benefits of this approach:**

* **Clean Components:** Components are focused solely on UI rendering and data presentation.
* **Reusability:** Custom hooks can be reused across multiple components needing similar data fetching.
* **Testability:** Hooks are plain JavaScript functions and are easier to test in isolation.
* **Maintainability:**  Changes to data fetching methods are isolated within the hooks, making updates easier.
* **Improved Code Organization:**  Clear separation of concerns leads to a more structured and maintainable codebase.

#### Example: `useFetch` Hook (`src/hooks/useFetch.js`)

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const json = await response.json();
        setData(json);
        setLoading(false);
      } catch (e) {
        setError(e);
        setLoading(false);
      }
    };
    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

Keep these "Don'ts" in mind as you code, and you'll be building awesome, reusable React components in no time! 
