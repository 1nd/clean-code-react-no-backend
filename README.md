# clean-code-react-no-backend
An example of directory structure and code snippets for using Clean Code Architecture on a React.js app that does not contain backend. 

This repository is my note from learning how to apply Clean Code Architecture for a React.js app. In this case, the React.js app:
- Uses Typescript.
- Uses GraphQL and Apollo as the GraphQL library.
- Has server-side rendering.
- Does not contain backend since its backend is provided by other app(s). The React.js app access the backend by calling its API via GraphQL.
- Has multiple UIs. In my example, I have separated UI for desktop website and mobile website. I decided not to adopt responsive web design in this example since this is for a case where your web app looks very different in desktop and mobile and may even have a completely different user journey, UI, and UX.

Since this is a learning note, it will contain mistakes and I do not intend it to be a knowledge source you can cite for your learning. I do not monitor the repository so Issues, Pull Requests, or any kind of comments may end up unanswered.

## The Example App

This repository contains a web app that has the following use-cases:
- Get all users (list users).
- Get a user.
- Add a new user (Register).
- Update/edit an existing user.

## Directory Structure

```
- src/
  - entities/    // Entity Layer in Clean Code Architecture
    - user.d.ts
  - useCases/    // Use Case Layer in Clean Code Architecture
    - shared/
      // ... Shared use-cases between desktop and mobile (if any) ...
    - desktop/
      - listUsers.ts
      - getUser.ts
      - addUser.ts
      - updateUser.ts
      // ... Other use case files specific to desktop ...
    - mobile/
      - listUsers.ts
      - getUser.ts
      - addUser.ts
      - updateUser.ts
      // ... Other use case files specific to mobile ...
  - adapters/    // Interface Adapters Layer in Clean Code Architecture
    - graphql/
      - apolloClient.ts    // Setting up the ApolloClient
      - shared/
        - listUsersQuery.graphql
        - getUserQuery.graphql
        - addUserMutation.graphql
        - updateUserMutation.graphql
        // ... Shared queries and mutations between desktop and mobile (if any) ...
      - desktop/
        - listUsersQuery.graphql
        - getUserQuery.graphql
        - addUserMutation.graphql
        - updateUserMutation.graphql
        // ... Desktop-specific queries and mutations ...
      - mobile/
        - listUsersQuery.graphql
        - getUserQuery.graphql
        - addUserMutation.graphql
        - updateUserMutation.graphql
        // ... Mobile-specific queries and mutations ...
    - infrastructure/    // Frameworks & Drivers Layer in Clean Code Architecture
      - react
        - shared/
          - index.tsx    // React.js bootstraping shared between desktop and mobile
          - utils/
            - useLocalStorage.ts    // A utility to use LocalStorage in desktop and mobile web (in React.js components). It is a React.js hook
      - desktop/
        - components/
          - UserList.tsx
          - UserDetails.tsx
      - mobile/
        - components/
          - UserList.tsx
          - UserDetails.tsx
      - server/
        - index.ts    // Server-side rendering with React.js
  - utils/    // Reusable utilities and helper functions. Not part of any layer in Clean Code Architecture.
    - localStorage.ts    // Logic for localStorage operation (including encryption)
    - validators/
      - isEmail.ts    // Validator to check if a string is valid email address


```

I decided to do one-to-one mapping from the Clean Code Architecture layers to the directory. It is for the simplicity of my learning. Of course, separation via directory structures do not mean the concerns are separated. Clean Code Architecture is about separation of concerns, not separation of files, or directories. But as a beginner that is still learning, having the directories mapped exactly how the layers separated makes it easier for me to learn and remember the layer separations.

Another concept that is crucial in Clean Code Architecture is Dependency Rule. An inner layer cannot depend on an outer layer. The order from the inner-most to the outer-most layers are:
- Entity Layer (directory name: "entities").
- Use Case Layer (directory name: "useCases").
- Interface Adapters Layer (directory name: "adapters").
- Frameworks & Drivers Layer (directory name: "infastructure").

So, for example, in `useCases` we cannot import/require/depend on something from `adapters` and `infrastructure`. If we indeed need it, we need to use Dependency Injection or other technique to not violate Dependency Rule which is crucial for a proper Separation of Concern.

The `utils` directory is not part of any Clean Code Architecture layer.
