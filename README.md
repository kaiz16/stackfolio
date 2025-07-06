<h1 align="center">Stackfolio</h1>

<h2 align="center">My personal portfolio of tech stacks and development strategies — covering everything from environment configs to CI/CD pipelines and starter templates. Feel free to use them as a reference for your own projects!</h2>

## 📝 Table of Contents

- [Introduction](#introduction)
- [Tech Stacks](#tech-stacks)
- [Code Strategies](#code-strategies)
  - [Ground Rules](#ground-rules)
  - [Code Formatting](#code-formatting)
  - [Naming Conventions](#naming-conventions)
  - [ENV Variables](#env-variables)
  - [TypeScript Config](#typescript-config)
  - [Git Strategies](#git-strategies)
  - [Sentry Integration](#sentry-integration)
- [Deploying on Google Cloud Platform](#deploying-on-google-cloud-platform)
  - [Setting up CI/CD Pipelines on Google Cloud Platform using GitHub Actions](#setting-up-cicd-pipelines-on-google-cloud-platform-using-github-actions)
    - [Important Notes](#important-notes)
    - [Stages of Deployment (Development, Production)](#stages-of-deployment-development-production)
    - [Requirements](#requirements)
    - [Step 1: Enabling Required APIs](#step-1-enabling-required-apis)
    - [Step 2: Creating a Service Account](#step-2-creating-a-service-account)
    - [Step 3: Generating a JSON Key](#step-3-generating-a-json-key)
    - [Step 1-3: Checklist Verification](#step-1-3-checklist-verification)
    - [Step 4: Enabling APIs](#step-4-enabling-apis)
    - [Step 5: Adding Secrets to GitHub Repository](#step-5-adding-secrets-to-github-repository)
    - [Step 4-5: Checklist Verification](#step-4-5-checklist-verification)
    - [Step 5: Creating a GitHub Action](#step-5-creating-a-github-action)
    - [Starter Templates](#starter-templates)
      - [Node MVC Starter](#node-mvc-starter)
      - [Hasura GraphQL Starter](#hasura-graphql-starter)
      - [Google Cloud Functions Starter](#google-cloud-functions-starter)
      - [React Starter](#react-starter)
      - [Next.js Starter](#nextjs-starter)
      - [Vue.js Starter](#vuejs-starter)
      - [Svelte Starter](#svelte-starter)
      - [Expo Starter](#expo-starter)
    - [Step 5: Checklist Verification](#step-5-checklist-verification)
    - [Step 6: Commit and Push Changes](#step-6-commit-and-push-changes)
- [Resources](#resources)
- [Contact](#contact)
- [Conclusion](#conclusion)
- [License](#license)
- [Contributing](#contributing)

## Introduction

Welcome to Stackfolio! This repository is a collection of my personal notes, resources, and tools that I use to build and maintain my projects. It includes everything from environment configurations to CI/CD pipelines, and serves as a reference for my tech stacks and development strategies. Starter templates are also included to help you get started quickly. Feel free to use them as a reference for your own projects!

## Tech Stacks

- **Frontend**: React, React Native/Expo, Svelte/SvelteKit, Vue, Next.js, Tailwind CSS, Typescript
- **Backend**: Node.js, Express, Dgraph, Hasura GraphQL, PostgreSQL, Supabase
- **CI/CD**: GitHub Actions, Docker, Google Cloud Platform

## Code Strategies

### Ground Rules

1. Always use type safe languages such as TypeScript, and Go. Don't use pure JavaScript unless you have a very good reason to do so. However, you may use pure JavaScript for serverless functions to automate small tasks.
2. When using npm, always use the [ESM module system](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules). No `require` statement please.
3. Don't need to always use [ESLint](https://eslint.org/). However, you may use it if you wish. If you do, make sure to use the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).
4. Always make sure your code is well-documented. You can use [GitHub Flavoured Markdown](https://github.github.com/gfm/) for this.
5. Always wrap your async code within a try/catch block. Refer [Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) for more information. Do not use `.then()` and `.catch()` for async code.

### Code Formatting

For code formatting, use **Prettier** with this [configuration](./configs/.prettierrc.yaml).

**To integrate Prettier into your project, follow the steps below:**

1. Install as a development dependency:

```bash
npm install --save-dev prettier
```

2. Create a `.prettierrc.yaml` file in the root of your project with the above configuration.

3. Automation with npm scripts: Add a script to your `package.json`:

```json
"scripts": {
   // Other scripts...
  "format": "prettier --write ."
}
```

By running `npm run format`, Prettier will automatically format your files, ensuring consistency throughout the project.

### Naming Conventions

For naming conventions, follow the standard conventions of modern software development. Here are guidelines I follow:

- **Use descriptive and meaningful names:** Be descriptive and use names that clearly convey the purpose of the object being named. This applies to tables, columns, relationships, functions, and any other elements in the applications.
- **Follow a consistent naming convention:** It's important to follow a consistent naming convention throughout your project. This makes it easier for developers to understand the codebase and helps to avoid confusion.
- **Naming Conventions:**
  - `snake_case` for table and column names in the databases.
  - `camelCase` for variables, functions and field names in GraphQL.
  - `PascalCase` for component names and export names in JavaScript/TypeScript.
  - `kebab-case` for CSS classes, CSS IDs, and HTML attributes and route names.
  - `ALL_CAPS` OR `SCREAMING_SNAKE_CASE` for environment variables and constants including GraphQL enum values.
- **Use plural nouns for table names:** Use plural nouns for table names. For example, use "payment_transactions" instead of "payment_transaction" for a table that stores payment transaction data. For GraphQL types, it should be singular + plural depending on the fields. For example, `paymentTransactions` to get multiple payment transactions and `paymentTransaction` to get a single payment transaction.
- **Schema-prefix table names:** Use a schema name as a prefix for table names to indicate the related data stored in the schema. For example, all tables in auth schema should have `auth_` as prefix. **Note: this does not apply to public schema.**
- **Separate ENUM Types schema:** Enum types should be stored under `types` schema when using SQL database. This way you can easily find and filter the enum types. Does not apply to NoSQL databases.
- **Use clear and concise descriptions:** Use clear and concise descriptions for routes, controllers, fields, relationships, and other elements in GraphQL/REST API. This makes it easier for developers to understand the purpose of each element.
- **Avoid abbreviations:** Avoid using abbreviations in names, as they can be confusing and make it harder to understand the purpose of the object being named. For example, use "transactions" instead of "txns" or "trans".

### ENV Variables

Different stages of the app's life require different settings, like database connection details or API keys. But I don't want to mix them up or expose sensitive data.

**Stages, Stages, Stages:** Usually, I use at least two stages or even three stages depending on project needs but mostly two is enough:

- **Development**: This is sandbox. It's where I test things out, break things, and then fix them.
- **Production**: This is the big stage. Real users, real data. Deployments here need to be clean. Hopefully, no breaking bugs or errors, production-ready code only.

**How to handle these different stages:** Use `.env` files, one for each stage:

- `.env.development` is for development stage.
- `.env.production` is for production stage.

**All these files should have the same structure.** Like, if `.env.development` has 3 variables, the production env file should too. Only the values change, depending on the environment.

**Always provide a `.env.example` file.** It gives other developers a glimpse of the key-value pairs needed to run the app. You can mostly copy over the stuff from `.env.development` here. But unlike other `.env` files, this one should NOT be ignored, so don't put it in `.gitignore`.

**Important**: All `.env` files (except `.env.example`) stay out of version control. Always double-check your `.gitignore` to ensure they're ignored.

**Tips for managing your `.env` files:**

- Keep sensitive data out of version control. Use `.gitignore` to prevent accidental exposure. Never commit env files with sensitive data.
- Regularly review and update your `.env` files to ensure they contain the latest values.
- Share `.env` files securely with your team, avoiding public channels.

### TypeScript Config

Config: [tsconfig.json](./configs/tsconfig.json) (Refer to comments in the file for more information)

[This](./configs/tsconfig.json) is the most common configuration I use. You may or may not need to adjust this according to your code. Refer to the [TypeScript Cheatsheet](https://www.totaltypescript.com/tsconfig-cheat-sheet) for more information.

### Git Strategies

Refer to this model here: [Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/)

### Sentry Integration

For error tracking in production, I use Sentry. Sentry is a tool that helps to track errors in apps deployed in production. I only use Sentry in production, not in development. This is because I don't want to track errors in development, as it can lead to a lot of noise and false positives. In production, I want to track errors that occur in the wild, so I can fix them quickly.

**Sample Configuration**

- Production: Trace sample rate: 0.25

Setting sentry is different for every language. As such, there's no "one-case fits all solution". Use the links below and follow the documentation to setup sentry in your application.

| Framework/Language | Guide                                                                      |
| ------------------ | -------------------------------------------------------------------------- |
| Vue                | [Vue Guide](https://docs.sentry.io/platforms/javascript/guides/vue/)       |
| React              | [React Guide](https://docs.sentry.io/platforms/javascript/guides/react)    |
| React Native       | [React Native Guide](https://docs.sentry.io/platforms/react-native)        |
| Svelte             | [Svelte Guide](https://docs.sentry.io/platforms/javascript/guides/svelte/) |
| Browser JS         | [Browser JS Guide](https://docs.sentry.io/platforms/javascript)            |
| Node.js           | [Node.js Guide](https://docs.sentry.io/platforms/node)                      |
| Python             | [Python Guide](https://docs.sentry.io/platforms/python/)                   |

## Deploying on Google Cloud Platform

All my projects are hosted on Google Cloud. Don't manually deploy applications to Google Cloud as there's always a risk of human error and inconsistencies. Instead, use CI/CD pipelines to deploy applications automatically. This ensures that the applications are always up-to-date and stable. Follow the steps below to set up CI/CD pipelines for your applications.

Refer to the following table for most commonly used platforms for different use cases on Google Cloud Platform:

| Platform        | Description                                             | Use-Cases                                             |
| --------------- | ------------------------------------------------------- | ----------------------------------------------------- |
| App Engine      | Deploy frontend SPAs or static websites. Good for SEO   | Marketing sites, landing pages, static blogs          |
| Cloud Run       | Run containerized apps without managing infrastructure. | APIs, microservices, backend APIs, SSR apps           |
| Cloud Build     | Automate build, test, and deploy.                       | CI/CD pipelines, automated testing                    |
| Cloud Storage   | Store and retrieve static assets globally.              | Image hosting, file backups, video storage            |
| Cloud Functions | Run serverless microservices for lightweight tasks.     | Webhooks, email triggers, notifications, file uploads |

You can find more information about these platform in the [Google Cloud Platform documentation](https://cloud.google.com/docs).

## Setting up CI/CD Pipelines on Google Cloud Platform using GitHub Actions

### Important Notes

The guide provided below is a general overview of how to set up CI/CD pipelines on Google Cloud Platform. The steps may vary depending on the specific requirements of your project and the technologies used. There is no one solution fits all when it comes to CI/CD pipelines. Every application is different and requires a different approach. I suggest you do some research on the platform you are deploying to and adjust the steps accordingly. Don't copy-paste and expect it to work. Always refer to the official documentation for the most accurate and up-to-date information.

### Stages of Deployment (Development, Production)

I mainly use two stages of deployment: development and production. The development stage is used for testing and debugging the applications, while the production stage is used for serving the applications to the end users. For example, when developing a web app with a React frontend and a Node.js backend, I would have a development and production environment for both the frontend and backend. This allows me to test the application in the development environment before deploying it to production.

Use Github workflows (aka Github Actions) for CI/CD pipelines.

**Again Stages, Stages, Stages:**

- **Development** - For development purposes: Used by developers & clients to test the applications and make fixes accordingly.
- **Production** - For production purposes: Used by real users. This is where the real money is made, and where the real problems arise.

### Requirements

1. UNIX compatible command line tools. For Windows, you can use the Git Bash command prompt or Windows Subsystem for Linux (WSL).
2. Github Repository with Admin access. Make sure you are able to add secrets to the repository.
3. Do double check your code and make sure there are no errors running locally. You don't want to fix the syntax or code-related errors during the deployment.
   - If you are using TypeScript, make sure to run `tsc` command to compile the code and check for any errors.
   - If you are using JavaScript, make sure to run `npm run lint` or `eslint .` to check for any linting errors.
   - If you are using Docker, make sure to build the Docker image and run it locally to check for any errors.
   - If you are using a framework like React, Vue, or Svelte, refer to the framework's documentation on how to build for production and run the application locally.
4. Make sure you have a Google Cloud Platform project with billing enabled. You can get a free trial with $300 credits from [Google Cloud Free Trial](https://console.cloud.google.com/freetrial?redirectPath=%2Fspanner&hl=en&facet_utm_source=%28direct%29&facet_utm_campaign=%28direct%29&facet_utm_medium=%28none%29&facet_url=https%3A%2F%2Fcloud.google.com%2Ffree).
5. Make sure that you have the .github/workflows folder in the root of your repository. If you don't have it, you can use the following commands to create the folders or you can create manually.

```bash
cd <your-repo>
mkdir -p .github/workflows
```

### Step 1: Enabling Required APIs

Before you set up CI/CD pipelines, you need to enable the Google Cloud Build API. This API is required for GitHub Actions to trigger deployments. Follow the steps below to enable the Cloud Build API:

1. Go to this link: [Cloud Build API](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com).
2. Select the correct project from the top navigation bar.
3. Click "Next" then "Enable" to enable the Cloud Build API for your project.
4. Once enabled, you should see a confirmation message saying "Enablement is already complete".

Next, enable the Container Registry API and Artifact Registry API. These APIs are required for storing and managing container images and artifacts:

1. Go to this link: [Container Registry API](https://console.cloud.google.com/flows/enableapi?apiid=containerregistry.googleapis.com).
2. Select the correct project from the top navigation bar.
3. Click "Next" then "Enable" to enable the Container Registry API for your project.
4. Once enabled, you should see a confirmation message saying "Enablement is already complete".
5. Go to this link: [Artifact Registry API](https://console.cloud.google.com/flows/enableapi?apiid=artifactregistry.googleapis.com).
6. Select the correct project from the top navigation bar.
7. Click "Next" then "Enable" to enable the Artifact Registry API for your project.
8. Once enabled, you should see a confirmation message saying "Enablement is already complete".

### Step 2: Creating a Service Account

Next, you need to create a service account that will be used by GitHub Actions to deploy your applications. A service account is a special type of account that grants permissions to perform actions on your behalf. Basically, it is a way to authenticate and authorize GitHub Actions to access your Google Cloud Platform project and deploy applications.

1. Go to this link: [Service Accounts](https://console.cloud.google.com/iam-admin/serviceaccounts).
2. Select your project.
3. Click on "Create Service Account".
4. Name your service account as `cicd-deploy`. It should automatically generate an email address for the service account. Write down that email and store it somewhere safe.
5. Click "Done".
6. It should take you back to the Service Accounts page. If the service account is not listed, refresh the page.
7. Click on the service account you just created.
8. Go to the "Permissions" tab.
9. Click on "Manage Access".
10. Under "Assign Roles", click "+ Add Role".
11. Grant the following roles to your service account:
    - App Engine Admin
    - Cloud Run Admin
    - Container Registry Service Agent
    - Artifact Registry Administrator
    - Cloud Functions Admin
    - Cloud Build Editor
    - Service Account User
    - Storage Admin
12. Click "Save".

### Step 3: Generating a JSON Key

Next, you need to generate a JSON key for the service account. This key will be used by GitHub Actions to authenticate and authorize the service account to access your Google Cloud Platform project.

1. Go to the Service Accounts page: [Service Accounts](https://console.cloud.google.com/iam-admin/serviceaccounts).
2. Select your project.
3. Click on the service account you created in the previous step.
4. Go to the "Keys" tab.
5. Click on "Add Key" then "Create New Key".
6. Select "JSON" as the key type and click "Create".
7. A JSON file will be downloaded to your computer. This file contains the credentials for the service account. Store it somewhere safe, you will need it later. **Think of this JSON file as a password. Once exposed, anyone can access your GCP project. So, make sure to keep it safe.**

### Step 1-3: Checklist Verification

- [ ] Service Account created with the name `cicd-deploy`.
- [ ] Service Account email address noted down.
- [ ] Service Account has the required roles assigned.
- [ ] JSON key file downloaded and stored securely.
- [ ] All of the above steps are done in the correct Google Cloud Platform project.

### Step 4: Enabling APIs

You need to enable the APIs in your GCP project depending on the platform you are deploying your application to. This might be different for every application. Here is a table to help you understand which APIs to enable.

| Platform        | APIs to Enable (Commas separated)                                         | Use-Cases                                             |
| --------------- | ------------------------------------------------------------------------- | ----------------------------------------------------- |
| App Engine      | App Engine Admin API,                                                     | Marketing sites, landing pages, static blogs          |
| Cloud Run       | Cloud Run Admin API, Google Container Registry API, Artifact Registry API | APIs, microservices, backend APIs, SSR apps           |
| Cloud Functions | Cloud Functions API, Cloud Resource Manager API                           | Webhooks, email triggers, notifications, file uploads |
| Cloud Storage   | Cloud Storage, Cloud Storage API                                          | Image hosting, file backups, video storage            |

To enable the APIs, follow these steps:

1. Go to the API Library page: [API Library](https://console.cloud.google.com/apis/library).
2. Select your project.
3. Search for the APIs you need to enable from the table above.
4. Click on the API and then click "Enable".

If you are having doubts about which APIs to enable, [contact me](#contact).

### Step 5: Adding Secrets to GitHub Repository

Now, you need to add the GCP and other secrets to your Github repository. This is required so that Github Actions can authenticate with your GCP project. Follow the steps below to add the JSON key to your Github repository.

1. Go to your Github repository.
2. Click on "Settings".
3. Click on "Secrets and variables" -> "Actions" -> "New repository secret".
4. Add the following secrets:
   1. `GCP_PROJECT_ID` - Your GCP project ID. You can find it in the server account JSON key you downloaded. Look for the `project_id` key in the JSON file. Copy the value and paste it here.
   2. `GCP_SA_KEY` - The content of the JSON key you downloaded in the previous step. Copy the entire JSON content and paste it here.
   3. `GCP_SA_EMAIL` - The email address of the service account you created in the previous step. Look for the `client_email` key in the JSON file. Copy the value and paste it here.
   4. While you're there, you can add other secrets too depending on your application. For example, if your app uses sensitive variables like database connection strings/passwords, you can add them here depending on its sensitivity. This is how you can keep your sensitive data secure and not expose them in your codebase. Any sensitive values in the `.env` file must be added as secrets. Here are some examples:
      - `DB_CONNECTION_STRING_DEV` - Connection string for the development database.
      - `DB_CONNECTION_STRING_PROD` - Connection string for the production database.

You can now access the secrets in your Github Actions by using `${{ secrets.KEY_NAME }}` where KEY_NAME is the name of the secret you added to your repository. For example, if you added the GCP_PROJECT_ID secret, you can access it by using `${{ secrets.GCP_PROJECT_ID }}` in your Github Action workflow file. More on this later.

### Step 4-5: Checklist Verification

- [ ] All required APIs enabled in the GCP project.
- [ ] GCP project ID added as a secret in the Github repository.
- [ ] GCP service account key added as a secret in the Github repository.
- [ ] GCP service account email added as a secret in the Github repository.
- [ ] Other required secrets added to the Github repository.

### Step 5: Creating a GitHub Action

To finish up the process, you need to create a Github Action. An action is a YAML file that contains the steps to be executed such as from building the application to deploying it. They run on a specific event in GitHub. In this case, let's create an action that runs on a push to the repository. This means that whenever you push code to the repository, the action will be triggered and it will deploy your application to Google Cloud Platform.

### Starter Templates

Provided below are some examples of Github Actions that I use. You can use them as a reference and adjust them accordingly to your application, see the comments in the code for more information.

#### Node MVC Starter

A Node.js MVC starter template that uses Express, TypeScript, and PostgreSQL (Supabase). Has a basic authentication system using JWT and a health check endpoint. Deployed on Google Cloud Run.

Link: [node-mvc-starter](https://github.com/yourusername/node-mvc-starter)

1. Create a `Dockerfile` in the root of your repository and add the config as seen in the example below.
   - This file is used to build the Docker image for your application.
2. Create a new workflow file in the `.github/workflows` folder and name it `[stage].deploy.yaml`, where `[stage]` is the stage name (eg: development) and add the script as seen in the example below.
   - This file is used to configure the Github Action. It tells Github Actions how to build and deploy your application.

#### Hasura GraphQL Starter

A Hasura GraphQL framework template that uses PostgreSQL (Supabase) as the database. Deployed on Google Cloud Run.

Link: [hasura-graphql-starter](https://github.com/yourusername/hasura-graphql-starter)

1. Create a `Dockerfile` in the root of your repository and add the config as seen in the example below.
   - This file is used to build the Docker image for your application.
2. Create a new workflow file in the `.github/workflows` folder and name it `[stage].deploy.yaml`, where `[stage]` is the stage name (eg: development) and add the script as seen in the example below.
   - This file is used to configure the Github Action. It tells Github Actions how to build and deploy your application.

#### Google Cloud Functions Starter

A minimal serverless function starter template that uses Node.js. Deployed on Google Cloud Functions.

Link: [google-cloud-functions-starter](https://github.com/yourusername/google-cloud-functions-starter)

1. Create a new workflow file in the `.github/workflows` folder and name it `[stage].deploy.yaml`, where `[stage]` is the stage name (eg: development) and add the script as seen in the example below.
   - This file is used to configure the Github Action. It tells Github Actions how to build and deploy your application.

#### React Starter
Coming soon!

#### Next.js Starter
Coming soon!

#### Vue.js Starter
Coming soon!

#### Svelte Starter
Coming soon!

#### Expo Starter
Coming soon!

### Step 5: Checklist Verification

- [ ] App Engine or Cloud Run or Cloud Functions configuration file created in the root of the repository.
- [ ] GitHub Action workflow file created in the `.github/workflows` folder.
- [ ] GitHub Action workflow file is named `[stage].deploy.yaml` where `[stage]` is the stage name (eg: development).
- [ ] GitHub Action workflow file contains the necessary steps to build and deploy the application.
- [ ] All required environment variables are set in the GitHub Action workflow file.
- [ ] All secrets are accessed using `${{ secrets.KEY_NAME }}` where `KEY_NAME` is the name of the secret added to the GitHub repository.

### Step 6: Commit and Push Changes

Before you push, use the checklists below to make sure you have followed all the steps.

- [ ] `.github/workflows` folder in the root of your repository.
- [ ] There is at least one Github Action file in the `.github/workflows` folder. It should end with .yaml.
- [ ] You have the following secrets in your Github repository:
  - `GCP_PROJECT_ID`
  - `GCP_SA_KEY`
  - `GCP_SA_EMAIL`
  - ... Other secrets according to your application needs (eg: database connection strings).
- [ ] You have adjusted the steps in the Github Action file according to your application.
- [ ] You have tested your code locally and have made sure there are no errors.

Once you have followed all the steps, push the changes to the repository to your test branch first. For example, if you have set the Github Action to run on the development branch, you can push the changes to the development branch. Then, go to the Actions tab in your repository. You should see the Github Action running.

Successful/failed Github Actions will be shown as a green check mark or a red cross mark.

- **Red cross**: This indicates that the Github Action has failed. You can click on the red cross to see the logs and debug the issue. [Contact me](#contact) if you are unable to resolve the issue.
- **Green check**: This indicates that the Github Action has succeeded. You can click on the green check to see the logs and verify that the application has been deployed successfully.

If an action succeeded, you can verify the services in the Google Cloud Console.

- App Engine: Go to Navigation Menu > App Engine > Services -> Click on the service name.
- Cloud Run: Go to Navigation Menu > Cloud Run > Services. Click on the service name. Copy "URL" and call any endpoint to see if the application is running.
- Cloud Functions: Go to Navigation Menu > Cloud Functions -> Click on the function name. Switch to the "Trigger" tab and copy the "URL". Call the URL to see if the function is running.

That's it! You have successfully created a CI/CD pipeline for your application. Congratulations!

## Resources

- [Google Cloud Platform](https://cloud.google.com/)
- [Docker](https://www.docker.com/)
- [Github Actions](https://docs.github.com/en/actions)
- [Cloud Run](https://cloud.google.com/run)
- [Cloud Functions](https://cloud.google.com/functions)
- [Cloud Build](https://cloud.google.com/build)
- [Cloud Storage](https://cloud.google.com/storage)
- [App Engine](https://cloud.google.com/appengine)

## Contact

If you have any questions or feedback, feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/realkaiz). Please be respectful and professional in your communication. I am always open to feedback and suggestions, and I will do my best to respond to your queries in a timely manner.

## Conclusion

This repository serves as a comprehensive guide to my tech stacks, code strategies, and deployment practices. It is designed to help you understand how I build and maintain my projects, and to provide you with the tools and resources you need to do the same. Feel free to explore the repository, contribute, and reach out if you have any questions or suggestions.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

If you would like to contribute to this repository, please read the [CONTRIBUTING](CONTRIBUTING.md) file for details on how to get started. Contributions are welcome and appreciated!
