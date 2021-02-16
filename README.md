# jigstack-guidelines
General code standards for Jigstack organization.

## Contributing
Jigstack's branch structure:

- main: main stable branch (lead dev is the main reviewer)
- develop: main staging branch (lead dev and prod manager will be the main reviewers)
- further branches: dev name such as kaue-cano, we can address details on merges by using PR title such as “bug fix: example feature” so the repository isn't polluted with branches. After merge, the dev can update this branch to catch up to the develop/main ones.

PRs must provide explanatory titles as well as any important notes in the description.

#### Good PRs

- Title-> Bug fix: Missing component on home / Desc-> Update this-component.tsx to include missing parameter that was breaking component loading.

- New feature: Implement editable campaign info / Update whole file-tree of campaigns (contracts, front and back) so users can edit metadata regarding campaigns. Note: front end yet to be covered.

#### Bad PRs

- Fix bugs / Fixed many bugs across the whole code. (not specific enough)

- Merge / Merged this into that. (must address the practical purpose, not the process)

Additionally, once the main branch is in check, you can also create a release by pushing a commit with a tag containing the 'v' character such as 'v0.0.1' while having the create-release.yml active in your repo.

## General

- Other useful github actions can be found in the /actions folder.

- A complete .gitignore can also be found in this repository.

- All third-party plugins/modules/libs MUST be specified in the README.md, including version and preferably links.

- Aim for end-to-end modularity, avoid monorepos at all cost.

- Sketch out interaction diagrams before/during development. Doing so makes it easier to visualize and, since documentation is already a requirement, leaving this task to the last moment can lead to overlooked details.

- Diagrams must accompany some sort of text-based explanation as well. Take this opportunity to create the best README.md you can. It isn't about length at all, it's about quality. If you can explain everything in three phrases instead of three paragraphs just as well, go for it.

- Comment as much code as you can, respecting the situation. A complex section deserves lengthy commentaries, while a simple one does just fine with a brief commentary.

- Delete unused and legacy branches so repositories aren't cluttered.


## Stack

### Web

- Node.

- TypeScript is favored over JavaScript for interfaces due to robustness.

- Don't forget to turn strict checks on for TypeScript.

- Use Babel Instead of tsc.

- Opaque types are your friend for avoiding bad variable passes.

- Prefer to use primitive types.

- Also prefer to use undefined as null type.

- Eliminate the use of any types, as well as for-in loops.

- No explicit type declarations for variables or parameters with literal values.

- There's no need to use custom TypeScript modules and namespaces.

- Don’t use non-null assertions after optional chain expressions or using the ! postprefix operator.

- Avoid using parameter properties in class constructors.

- Never use require() to import modules, and avoid aliasing 'this' or types in general.

- Don’t throw literals as exceptions.

- React coupled with Redux is preferred over any other JS framework, but higher-level libraries based off of React are also welcome, such decision should be commented upon though.

- Coverage to a max, especially on vital parts of the UX. Choice of testing framework should also be commented upon. Standard React tests do the job just fine.

- Use ES6 syntax as needed to clean up your code, but don’t over re-factor it.

- It's a good idea to start static.

- Always a good idea to move components into their own folders and you can configure a package.json file per each folder.

- Another good idea is to name the components after they're built so the name is more fitting.

- You don’t always need a class component, eespecially if you aren’t managing state.

- Avoid className and Style in your components.

- Simplify hooks as much as possible.

- Only call hooks from react components.

- Put CSS in JavaScript/TS directly.

- Avoid objects inside setStates.

- Avoid unnecessary divs.

- Create tiny components that just do one thing so they're more reusable.

- Build your UI/logic component library as soon as possible so everything is more organized and easier to work with.

- Design the data model with as many people as you can so it's more future-proof.

- Avoid over-engineering and over-optimization, that's what tests are here for. Simplicity and progress are key.

- Validate inputs and handle errors.

- Implement health check endpoints and logging.

- Implement versioning for all services.

- Separate stateful aspects from rendering.

- During development, use react developer tools over standard chrome inspect.

- Use objects as payloads for functions.

- Utilize [prop-types](https://www.npmjs.com/package/prop-types) as much as you can so you have more control over the application.

- Use [ESLint](https://eslint.org/) to keep your code nice and tidy.

- [Prettier](https://prettier.io/) is your friend on correctly and nicely formatting your code.

- You can also use [Sonarlint](https://www.sonarlint.org/) and [Husky](https://www.npmjs.com/package/husky) for suggestions on how to improve your formatting approaches.

- For more guidelines please check [TypeScript Docs](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html).


### Contracts

- We should aim at the highest stable version of Solidity as much as we can. 0.6.X should be avoided and 0.7.1 upwards is acceptable.

- Truffle is the standard coding environment, although Hardhat is also encouraged. Latest stable version always.

- Contracts should not be deployed using Remix, it's only usable in testing scenarios.

- OpenZeppelin infrastructure is good.

- Reuse, don't copy and paste.

- Follow _internalNaming standards.

- All code must follow [natspec](https://docs.soliditylang.org/en/develop/natspec-format.html) comment standard to avoid vulnerability and make expansion easier.

- All public components must have 100% test coverage.

- Truffle tests are great and are the standard, but Waffle is also welcomed, especially in a Hardhat environment.

- Coupling tests with [solidity-coverage](https://github.com/sc-forks/solidity-coverage) provides great visualization into the code coverage. 

- [Slither](https://github.com/crytic/slither/wiki/Printer-documentation) is your best friend when coming up with diagrams and automated analysis.

- Slither also MUST be part of CI and quality control before any goal is checked as done/in-review.

- [Echidna](https://github.com/crytic/echidna) tests should be applied as much as possible and also be part of CI.

- [Manticore](https://github.com/jigstack-dev/manticore) is also a great tool for static code analysis and should be employed before any major deployment.

- Contracts must be pausable.

- Preferably, they should also be upgradable following proxy architecture (logic and data separation) from big projects such as Uniswap, Compound, Curves and YFI.

- Migrations are also acceptable, but not that sustainable in the long term, so we should keep it for punctual/short-term cases.

- Data scraping and storage is extremely important on upgrades and migrations, so this is a must either way.

- Another thing to keep in mind, especially on upgradable contracts is how blockchain data-types are structured. Mappings, for instance, should follow a very rigid order so it isn't mixed up on redeploys.

- All code MUST take migrations, preferably upgrades, into consideration. Locally storing data helps (and is a great practice), but burning gas from unnecessary deploys is something we want to avoid, both for ourselves and for our public image.

- [OpenZepellin Defender](https://openzeppelin.com/defender/) is our go to DevSecOps tool.

- A good practice is to keep only the necessary on-chain. For instance, we can store/fetch from blockexplorer data locally and just compare it with the blockchain instead of doing everything on-chain.

- All important events must be logged. This is critical for monitoring.

- Before any production deployment we must have a tested incident plan.

- The assert function should only be used to test for internal errors, and to check invariants.

- The require() function should be used to ensure valid conditions, such as inputs, or contract state variables are met, or to validate return values from calls to external contracts. 

- [Check Effects Interactions](https://fravoll.github.io/solidity-patterns/checks_effects_interactions.html) are a great way to strengthen security.

- .transfer() and .send() functions should be substituted with .call() + Checks-Effects-Interactions instead.

- Code should be handling unbreaking false throws.

- Contracts should not assume that its initial state contains a zero balance as it isn't always the case and can break your code.

- Beware of negation of the most negative signed integer (negative(-255) doesn’t negate).

- Don't rely on second/third-party interactions to proceed the contract flow since it can lead to unexpected error/vulnerabilities.

- Never use tx.origin or extcodesize.

- Check OpenZepellin Defender Advisor against any code to be deployed.

- Follow [this repo](https://github.com/jigstack-dev/smart-contract-best-practices) for further, growing, best practices.