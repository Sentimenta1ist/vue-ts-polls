# Polling Web App (Vue 3 + TypeScript)

A small single-page polling site: registered users create questions with predefined answers and vote; the author can delete a poll, and an admin can see all users and delete any poll. Built for two web UI labs, with unit and end-to-end tests.

Year: 2023 · Stack: Vue 3, TypeScript, Vuex 4, Vue Router 4, Bootstrap 5, Jest + Vue Test Utils, Cypress · Context: university coursework (KPI, master's program), labs "Client-side web application" and "Testing the client side"

## What's inside

- `src/views/` - pages: `HomePage` (poll list), `CreateQuestionPage`, `QuestionPage` (vote and results), `LoginPage`, `RegisterPage`, `AccountPage`, `UsersPage` (admin), `AboutPage`. Routes are in `src/router/index.ts`.
- `src/components/` - `QuestionList.vue` and `UsersTable.vue`.
- `src/cls/model/` - plain TypeScript model classes: `User`, `Users`, `Question`, `Questions`, `Answer`.
- `src/store/index.ts` - Vuex store (users, questions, current user) with actions and mutations for add / delete user, add / delete question, login, logout. There is no backend: data is in memory and seeded from `src/assets/mock-users.json` and `mock-questions.json`.
- `tests/unit/` - 9 Jest specs, one per page or component.
- `tests/e2e/specs/` - Cypress specs per page (`separate-pages/`) and multi-step flows in `complex-actions/scenarios.spec.js`.

## Interesting bits

- `src/cls/model/Question.ts`: votes are stored as lists of voter logins per answer; `isAlreadyVote` and `findVotedAnswerId` enforce one vote per user and show the user's previous choice.
- `tests/e2e/specs/complex-actions/scenarios.spec.js`: full user flows are scripted: register, log in, vote on question 1 and see the count; register, log in, log out; log in as admin, create a question and delete it.
- `src/store/index.ts`: login is checked against the in-memory user list, and passwords are stored and compared in plain text. This was acceptable for a lab without a backend, not for anything real.

## Run

Verified in this container (Node 22, in a clean copy):

```
npm ci            # add CYPRESS_INSTALL_BINARY=0 to skip downloading the Cypress binary
npx vue-cli-service test:unit
npx vue-cli-service build --skip-plugins @vue/cli-plugin-eslint
npm run serve     # dev server, not re-tested
```

Results: 18 of 19 unit tests pass. `about-page.spec.ts` fails because it still expects the text "created by Yermolenko Denys" while the page names a different author. A plain `npm run build` fails because the repository has no ESLint config file, hence the flag above. The Cypress e2e tests were not run (`cypress.json` has a typo in `baseUrl`: `localohost`).


## Note

Written in 2023, before AI coding assistants. Kept as-is for history.
