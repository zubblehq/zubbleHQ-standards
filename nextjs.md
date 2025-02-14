## Code Structure

```
project-root/
├── app/             # Next.js App Router (recommended)
│   ├── (group-name)/ # Route groups for organization
│   │   ├── layout.js
│   │   ├── page.js
│   │   └── components/
│   ├── api/         # API route handlers
│   ├── layout.js      # Root layout
│   └── page.js        # Root page
├── pages/           # (Legacy) Next.js Pages Router (if used)
│   ├── api/         # (Legacy) API routes
│   └── ...
├── components/      # Reusable React components
│   ├── ui/          # UI components (buttons, inputs, etc.)
│   ├── forms/       # Form components
│   └── ...
├── styles/          # Global styles and CSS modules
│   ├── globals.css  # Global CSS (use sparingly)
│   ├── components/  # CSS Modules for components (optional)
│   └── ...
├── public/          # Static assets (images, fonts, etc.)
├── lib/             # Utility functions and helper logic
├── utils/           # (Alias for lib/) Utility functions and helper logic
├── config/          # Configuration files
├── hooks/           # Custom React hooks (useFileName.ts convention)
├── context/         # React Context providers (if used)
├── store/           # State management store (if using a library)
├── tests/           # Unit and integration tests
├── e2e/             # End-to-end tests (optional)
├── .env.example     # Example environment variables
├── .env.local       # Local environment variables (not committed to Git)
├── next.config.js   # Next.js configuration
├── package.json
├── README.md
├── tsconfig.json    # TypeScript configuration (if using TypeScript)
└── CODE_STYLE.md    # Detailed coding and naming conventions
```

## Coding and Naming Conventions

We follow consistent coding and naming conventions to ensure code maintainability and readability. Please refer to the separate [CODE_STYLE.md](CODE_STYLE.md) file for a detailed breakdown of our conventions.

## Key Conventions:

- **File and Folder Names:** kebab-case (lowercase with hyphens). Folder names are also lowercase.

    Example: `user-profile.js`, `product-details/`, `api/create-user/route.js`

- **React Components:** PascalCase.

    Example: `UserProfile`, `ProductCard`, `SubmitButton`

- **Variables and Functions:** camelCase.

    Example: `userName`, `productPrice`, `handleButtonClick`

- **Constants:** UPPER_SNAKE_CASE.

    Example: `API_ENDPOINT`, `MAX_ITEMS_PER_PAGE`

- **Custom Hooks:** `usePascalCase.ts` (e.g., `useUserData.ts`). The folder `hooks/` is lowercase.

    Example: `useUserData.ts` (in `hooks/` folder)

- **CSS Classes:** kebab-case.

    Example: `.product-card`, `.submit-button`

- **API Routes:** kebab-case for route segments.

    Example: `/api/products`, `/api/users/user-profile`

## Tools for Enforcement:

- **ESLint:** For code linting and style checking.

- **Prettier:** For automatic code formatting.

## Server Actions Guidelines

Server Actions in Next.js allow you to execute server-side logic directly from your React components. Follow these guidelines when implementing Server Actions:

### When to Use Server Actions:

*   **Form Handling:** Ideal for handling form submissions that require server-side processing (database updates, sending emails, etc.).
*   **Data Mutations:** For any operations that modify data on the server (creating, updating, deleting).
*   **Security Sensitive Operations:** Keep sensitive logic on the server to prevent exposure on the client-side.
*   **Simplified Data Fetching and Mutations:** Offers a more streamlined way to interact with the server compared to traditional API routes for certain use cases.

### Naming Conventions for Server Actions:

*   **camelCase function names:** Use descriptive camelCase names for your Server Action functions.
    *   Example: `createUser`, `updateProduct`, `deleteComment`
*   **Clearly indicate the action's purpose:** The name should clearly convey what the Server Action does.

## Location of Server Actions:

**Component-Level (Co-located):** Define Server Actions directly within the component where they are used, especially for actions specific to that component. This can improve code locality and readability for smaller actions.

```javascript
// app/components/MyForm.js
async function handleSubmit(formData) {
  'use server'; // Mark as Server Action

  // Server-side logic here (e.g., database interaction)
  const name = formData.get('name');
  console.log('Form submitted:', name);
  // ...
}

export default function MyForm() {
  return (
    <form action={handleSubmit}>
      {/* ... form inputs */}
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Separate Utility Files (for Reusable Actions)**: For actions that are reusable across multiple components or represent core application logic, create dedicated files (e.g., in `lib/actions/` or `utils/actions/`).

```javascript
// lib/actions/user-actions.js
'use server';

export async function createUser(userData) {
  // ... server-side logic to create a user
}

export async function updateUserProfile(userId, profileData) {
  // ... server-side logic to update user profile
}
```

Then import and use these actions in your components:
```javascript
import { createUser } from '@/lib/actions/user-actions';

async function handleSubmit(formData) {
  'use server';
  const name = formData.get('name');
  await createUser({ name });
  // ...
}

```
## Error Handling and User Feedback

**try...catch blocks:** Wrap Server Action logic in `try...catch` blocks to handle potential errors gracefully on the server.

**Return Values:** Server Actions can return values to the client. Use this to return success/error messages or data to update the UI.

**useFormState Hook:** Use the `useFormState` hook to manage form state and display error messages to the user directly within the form. This provides a better user experience for form submissions.

```javascript
'use client'; // useFormState is client-side

import { useFormState } from 'react-dom';

async function handleSubmit(prevState, formData) {
  'use server';
  try {
    // ... server logic
    return { success: true, message: 'User created successfully!' };
  } catch (error) {
    return { success: false, message: 'Error creating user.' };
  }
}

export default function MyForm() {
  const [state, formAction] = useFormState(handleSubmit, { success: null, message: '' });

  return (
    <form action={formAction}>
      {/* ... form inputs */}
      <button type="submit">Submit</button>
      {state?.message && <p className={state.success ? 'success' : 'error'}>{state.message}</p>}
    </form>
  );
}
```

## Best Practices:

1.  **Keep Actions Focused:** Each Server Action should ideally perform a single, well-defined task.

2.  **Test Server Actions:** Write unit tests or integration tests for your Server Actions to ensure they function correctly and handle errors as expected.

3.  **Document Server Actions:** Document the purpose, inputs, and outputs of your Server Actions, especially if they are reusable or part of a larger API surface.

4.  **Use Client-Side Transitions (where appropriate):** For actions that cause UI changes, consider using client-side transitions (e.g., `router.push` after a successful Server Action) for a smoother user experience.