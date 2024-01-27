# 🚀 Micro-frontend with Module Federations (VITE + REACT)

![Logo](https://raw.githubusercontent.com/ProshenjitKarmakar/micro-front/main/micro-frontend-architecture.webp)


**Introduction :** Micro-frontends is an architectural style where a frontend application is broken down into smaller, independent "micro" applications, each responsible for a specific feature or functionality. In this artical, I'll implement Micro-frontends using [Module Fedaration](https://github.com/originjs/vite-plugin-federation) (VITE + REACT). I'll go through step by step so that you can implement a micro-frontend architecture from scratch.

## 💬 First we will go through some topics?
 - **1. What is Micro-frontend? How does it works?** 🤔
 - **2. What is Module Fedaration? How does it works?** 🤔
 - **3. How to implement module federation using Vite?** 🤔
 - **4. Step by step installation procedure.**
 - **5. Module federation configuration setup for implementing micro-frontend.**
 - **6. Parent Module Configuration.**
 - **7. Child or Remote Module Configuration.**
 - **8. How to use child or remote modules inside the host or parent application**
 - **9. Some concepts we need to take care.**
 - **10. Why and when we have to use Micro-frontend**
 - **11. Conclusion**


## 1. What is Micro-frontend? How does it works?
Micro Frontends is an architectural pattern that draws inspiration from microservices, focusing specifically on the front-end layer. It involves breaking down a monolithic front-end application into smaller, loosely coupled, and independently deployable components.

The key principles of micro frontends include componentization, independent development and deployment, technology diversity, and team autonomy. These principles enable teams to work on different parts of the application simultaneously, leveraging their preferred technologies and frameworks.

Micro-frontends are an architectural approach to developing web applications that involves breaking down a monolithic frontend into smaller, more manageable pieces, known as micro-frontends. The main goal to implement micro-frontend is to enable independent development, deployment, and maintenance of these smaller units.

**Development :**

- Development teams can work on different micro-frontends concurrently.
- Each team is responsible for a specific micro-frontend, allowing for parallel development cycles.
- Teams can use different technologies or frameworks based on the requirements of their micro-frontend.

**Isolation :**

- Micro-frontends are isolated from each other at the code level. They can have their own codebase, frameworks, and libraries.
- Communication between micro-frontends is typically done through well-defined APIs, events, or a shared state management system.

**Build and Deployment :**

- Each micro-frontend is built and deployed independently. This allows for continuous integration and continuous delivery (CI/CD) for individual features.
- Micro-frontends can be hosted and served independently or as part of a larger application.

**Integration :**
- Integration of micro-frontends happens at runtime. This is often facilitated by a module federation system.
- The main application or shell loads the micro-frontends dynamically, either during initialization or in response to user actions.
- The integration process involves combining the different micro-frontends into a cohesive user interface.


**Routing and Navigation :**
- Micro-frontends can be linked together through a central routing mechanism or by using a combination of client-side and server-side routing.
- Navigation between different micro-frontends is handled seamlessly, giving the illusion of a single, integrated application.

**Communication :**
- Micro-frontends need to communicate with each other to provide a cohesive user experience.
- Communication can be achieved through various means, such as events, APIs, shared state, or a combination of these.
- Careful consideration is given to maintaining a clean interface between micro-frontends to avoid tight coupling.

**Scaling and Flexibility :**
- Micro-frontends enable better scalability and flexibility in development and deployment.
- Teams can scale independently based on the demand for specific features or modules.
- Updates or changes to one micro-frontend do not necessarily affect others, reducing the risk of unintended side effects.


## 2. What is Module Fedaration? How does it works?
Module Federation is a feature provided by Webpack, a popular JavaScript module bundler, that enables you to dynamically load and run code from different bundles at runtime. It's particularly useful in a micro-frontends architecture, where multiple independently developed and deployed applications need to work together to form a cohesive user interface.

**Dynamic Loading of Remote Modules :**
- In a traditional monolithic application, all code is bundled together and served as a single package. Module Federation allows you to split your application into smaller, independently deployable bundles.
- With Module Federation, you can dynamically load modules from different bundles at runtime. This means that parts of your application can be loaded on-demand, improving performance and reducing the initial loading time.

**Exports and Remotes :**
- Modules in Webpack can export functionality using the export keyword. With Module Federation, you can mark certain parts of your codebase as "exposed" or "remotes."
- The "exposed" modules are parts of your codebase that you want to share with other applications, while "remotes" are references to external applications that expose certain modules.

**Configuration :**
- To use Module Federation, you need to configure your Webpack build. Each application (or bundle) needs to specify which modules it exposes and which modules it consumes from other applications.
- The configuration includes information about the shared dependencies and the entry points for remote applications.

**Remote Entry :**
- When an application is configured with Module Federation, it generates a remote entry file. This file includes metadata about the exposed modules and can be used by other applications to dynamically load those modules.
- The remote entry file is typically hosted on a server accessible to other applications.

**Dynamic Import :**
- To use a remote module, an application can use dynamic imports to load the module at runtime. This is done using the import() function, which returns a Promise that resolves to the module's namespace.

**Mounting and Integration :**
- Once a remote module is loaded, you can use its functionality and integrate it into your application. This may involve mounting components, calling functions, or using services exposed by the remote module.
- Care should be taken to ensure a clean interface between different parts of the application to avoid tight coupling.


## 3. How to implement module federation using Vite? 
Vite does not have native support for Module Federation, you may need to use Webpack to achieve this. Below are generic steps you can follow using Webpack.
- **Create Vite Projects :** Set up separate Vite projects for each micro-frontend. Each project should have its own Vite configuration.
- **Install Webpack and Module Federation Plugin :** Install Webpack and the Module Federation plugin in each Vite project.
- **Configure Webpack for Module Federation :** Update the webpack.config.js file in each micro-frontend project to include the Module Federation plugin.
- **Import Micro Frontends Dynamically :** In your main application, import the micro-frontends dynamically using dynamic imports.
- **Mount Micro Frontends :** Inside the micro-frontends, create a mount function that accepts a container element and mounts the micro-frontend components.
- **Run Micro Frontends :** Start the development servers for each micro-frontend and the main application.

## 4. Step by step installation procedure : 
- **First, let's create our directory using :**

    ```bash
    mkdir micro-frontend && cd micro-frontend
    ```
- **Install Vite globally using npm :**
    ```bash
    npm install -g create-vite
    ```
- **Create a Vite Project with React :** Create a new Vite project and choose the React template:

    ```bash
    create-vite my-microfrontend --template react
    cd my-microfrontend
    ```
- **Repeat for Additional Micro-frontends :** If you have multiple micro-frontends, repeat immediate step to create additional Vite projects for each micro-frontend.
- **Install the necessary Webpack and Module Federation packages :**
    ```bash
    npm install --save-dev webpack webpack-cli @module-federation/webpack-plugin
    ```
- **Configure Shared Dependencies :** If you have shared dependencies between your micro-frontends, configure them in your Vite project's `package.json` file. Configure the child and parent application port and others as you needed.
    ```javascript
    "scripts": {
        "dev": "vite --port 3001 --strictPort",
        "build": "tsc && vite build",
        "preview": "vite preview --port 3001 --strictPort",
    }
    "dependencies": {
        "react": "^16.2.0",
        "react-dom": "^18.2.0",
        "react-router-dom": "^6.14.2",
    },
    "devDependencies": {
        "@originjs/vite-plugin-federation": "^1.2.3",
        "@types/node": "^20.4.5",
        "@types/react": "^16.2.15",
        "@types/react-dom": "^18.2.7",
        "typescript": "^5.0.2",
        "vite": "^4.4.5"
    },
    ```
**Integrate Micro-frontends in Main Application :** In your main application (which may be another Vite project or any other application), import and integrate the micro-frontends as needed. This integration might involve dynamic imports and mounting components from the micro-frontends. All configuration are mentioned below.

## 5. Module federation configuration setup for implementing micro-frontend :
There are always minimum two places where we need to configure to get the module federation functionality. One configuration should be applied on the remote application where we have the components to be shared to tell Vite that what are the components that will be shared as modules and what is the entry name for the build. Other Configuration should be applied on the host application side where we gonna use the federated modules.

## 6. Parent Application Configuration :
```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default ({ mode }) => {

    return defineConfig({
        plugins: [
            react(),
            federation({
                name: 'parent',
                remotes: {
                 child: 'http://localhost:3001/assets/remoteEntry.js',
                },
                shared: ['react', 'react-dom', 'react-router-dom'],
            }),
        ],
        build: {
            target: 'esnext',
            modulePreload: false,
            minify: false,
            cssCodeSplit: false,
        },
    });
};

```
There are two ways to configure host application to bring in remote modules. One is directly referring to the URL of the remote module entry file. Second is dynamically populate the reference. Both has a smilier set of properties as to the remote side except for the property remotes . As Nameand Shared property are same as explain above following is the explanation for remotes property.

- **Remotes :** Remotes holds references to entry files of remote modules. Following are the examples of how we can use remotes property. More details about [Remotes](https://github.com/originjs/vite-plugin-federation) will found here.

- **Get the remote component using URL :**

    ```bash 
    remotes: {
        sharedComp: 'http://localhost:3001/assets/remoteEntry.js',
    },
    ```
- **Get the Remote component by applying URL dynamically :**
    ```bash 
    remotes: [
        {
            sharedComp: {
                external: `Promise.resolve(window.remoteURL)`,
                from: 'vite',
                externalType: 'promise',
            },
        },
    ],
    ```
- **External :** Can be the address of the remote module, basically the URL or a Promise<string>. More details about [Remotes](https://github.com/originjs/vite-plugin-federation) will found here.
    ```bash
    remotes: {
        home: {
            external: `Promise.resolve('your url')`,
            externalType: 'promise'
        },
    },
        
    // or from networke
    remotes: {
        remote-simple: {
            external: `fetch('your url').then(response=>response.json()).then(data=>data.url)`,
            externalType: 'promise'
        }
    }
    ```

- **From :** will let the Vite know that from where the remote module coming from. Whether it’s coming from Webpack or Vite. More details can be found here.

- **ExternalType :** will set what type of external reference will be used in external property. So value for this can be url or promise . More details can be found here.





## 7. Child/Remote Application Configuration :

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
    plugins: [
        react(),
        federation({
             name: 'Child',
             filename: 'remoteEntry.js',
             exposes: {

                 // all imports here -------

                 './Button': './src/components/button/index.tsx',
                 './Home': './src/views/home/index.tsx'
             },
             shared: ['react', 'react-dom', 'react-router-dom'],
        }),
    ], 

    build: {
        target: ['edge90', 'chrome90', 'firefox90', 'safari15'], // browsers
        modulePreload: false,
        minify: false,
        cssCodeSplit: false,
    },
});
```
**Name :** Name is the module name to be given to the javascript module that will be built including shared components. This is required. More details about [Name](https://github.com/originjs/vite-plugin-federation) will found here.

**File Name :** Name is the Filename of the entry file for the javascript module. This is not required though as the default value for this will be `remoteEntry.js`. More details about [File Name](https://github.com/originjs/vite-plugin-federation) will found here.

- **Exposes :** Exposes is where we need to list down the components that we are going to expose as remote component with the remote module. More details about [Exposes](https://github.com/originjs/vite-plugin-federation) will found here.
    ```bash
    exposes: {
    // 'externally exposed component name': 'externally exposed component address'
        './remote-simple-button': './src/components/Button.vue', 
        './remote-simple-section': './src/components/Section.vue'
    },

    // If you need a more complex configuration

    exposes: {
        './remote-simple-button': {
            import: './src/components/Button.vue',
            name: 'customChunkName',
            dontAppendStylesToHead: true
        },
    },
    ```

**NOTE: All the exposing components should export the react component as a default export. Otherwise it will not be able to integrate without an issue from the Host application side as there is no enough details to import individual export from a react component.**

- **Shared :** So this is bit of a complex property. When it comes to libraries like react we need to have one instance shared between all the libraries to handle states of components without an issue. So when we are working with remote modules we need a way to use one react instance in both remote module and the host application. To achieve this we need tell that what libraries will be shared between host and remote modules. This property will enable us to list those libraries. So we need to add this property in both host side configuration and remote application side configuration and add the same list of libraries in both side to be indicate what need to be shared. More details about [Shared](https://github.com/originjs/vite-plugin-federation) will found here.

## 8. How to use remote module inside the host application
There are two ways to use remote modules in a component in the host application.

- **As a Static Import :** We can always add the remote module as a static import inside a react component.
    ```bash
    import Button from 'sharedComp/Button';

    function App() {
    return (
        <div className="App">
        <div className="card">
            <Button />
        </div>
        </div>
    )
    }

    export default App;
    ```
This is fine but as to the performance and reliability this is not the most promising way. I have faced few issues when comes to this method. Also using this its hard to handle network level issues.

- **As a Lazy loaded module :** This is to load the remote module using lazy loading to up the performance and network issue handling.

   ```bash
    import {lazy, Suspense} from 'react';

    const Button = lazy(() => import('sharedComp/Button') as any);
    
    function App() {
        return (
            <div className="App">
                <div className="card">
                    <Suspense fallback={<div>Loading...</div>}>
                    <Button />
                    </Suspense>
                </div>
            </div>
        )
    }

    export default App;
    ```
This is my recommended way of using remote modules as this give us ability to handle network related issues and performance related issues.

***NOTE: if you prefer to implement this using typescript make sure you add a custom type declaration file to your src root and add the name of the remotes config as a module. file can be named something like `module.d.ts`***
```bash
declare module 'sharedComp/*' {};

specifically :
declare module 'sharedComp/Button';
```
## 9. Some concepts we need to take care.
There are few things we need to keep in mind when we are running the both Host and Remote applications.

- Remember to run the remote application and host application in Preview mode when developing instead of Development mode to get the file serving working. Otherwise if we ran applications on dev mode because of dev server doesn’t serve files we cannot get the module federation working in dev mode.

- Also remember when a component is shared it will be shared as a javascript module and not as an application. The responsibility of an application will be carried out by the host application that will bring in and uses the shared module. Hence when remote component uses environment variable or any other process related data will be pushed through host application to remote components. So when using remote component remember that host application will always be the platform that this remote component running on. Because even though remote component running on a separate server remember that remote server only serving a file at this point when comes to module federation and nothing more. You still can see the remote server side application running using URL but from host application side we are only referencing to the shared javascript module entry .js file. So make sure to provide every process related data from Host application even though they are provided in you remote application side.


## 10. Why and when we have to use Micro-frontend
Micro-frontend is to improve the scalability, flexibility, and maintainability of the frontend codebase. Here are some reasons why and when you might consider using micro-frontends:

- **Independent Development and Deployment :** Team Autonomy: Micro-frontends allow different teams to work on separate features or modules independently, making it easier to release updates without coordination across the entire application.

- **Reuse and Composition :** Micro-frontends encourage the reuse of components across different parts of the application. This can lead to a more modular and maintainable codebase.

- **Team Collaboration :** Cross-Functional Teams: Micro-frontends align well with the concept of cross-functional teams, where each team is responsible for end-to-end development and maintenance of specific features or modules.

## 11. Conclusion
In conclusion, the adoption of micro-frontends can provide several benefits but comes with its own set of challenges. Here's a summary of key points to consider:

**Advantages of Micro-frontends :**

- Team Autonomy: Enables independent development and deployment, allowing teams to work autonomously on different parts of the application.

- Technology Diversity: Supports the use of diverse frontend technologies, allowing teams to choose the best tools for their specific requirements.

- Scalability: Scales development efforts by facilitating parallel work on isolated parts of the application, especially useful as teams grow.

- Isolation and Decoupling: Promotes code isolation, reducing the risk of bugs spreading across the entire application, and facilitates easier maintenance and refactoring.

- Flexible Technology Upgrades: Allows for the independent upgrade or replacement of specific parts of the application, facilitating the adoption of new technologies.

- Reduced Development Bottlenecks: Facilitates parallel development, reducing development bottlenecks and accelerating the overall development process.

**Challenges and Considerations :**

- Communication Between Micro-frontends: Managing communication and coordination between micro-frontends can be challenging and requires careful planning.

- Consistent User Experience: Ensuring a consistent user experience across different micro-frontends may require additional effort and coordination.

- Shared Resources: Handling shared resources, such as stylesheets and dependencies, needs careful consideration to avoid conflicts.

- Increased Complexity: Introduces additional complexity, and the decision to adopt micro-frontends should be based on the specific needs and goals of the project.

- Learning Curve: Teams may need to adapt to new practices and tools, which could have a learning curve.
