# ** TABLE OF CONTENT **

    1. Figuring out how to add the toggle button in the Create New Bidder form under the service categories, and also updating it in the state.
    2. Figuring out how to integrate devextreme datagrid.
    4. Figuring out how to style devextreme datagrid to my taste.
    5. Error anytime I create a remote repository with a readme and try to push from an existing local repository.
    6. Figuring out how to integrate WebShareApi.

### Issue: Figuring out how to add the toggle button in the Create New Bidder form under the service categories, and also updating it in the state.

### Solution: So I installed all fontawesome dependies and imported them. I also increased using tailwind "text". To update the state I gave the icon a data-icon-name attribute and set it to the name it represent in the state. For instance "data-icon-name="insurance". Since their initial value is a boolean and it is set to false, I used that to check for whether the toggle is on or off using ternary operation. The fontawesome icon has two state, faToggleOn and faToggleOff. Then I created a helper function handleToggleOnClick(e, "serviceCategories") which receives the event and then the category in the form. Did this because of the way my state is structured.

    Here is the structure of my state:
    const [createNewBidderFormValues, setCreateNewBidderFormValues] = useState({
        general: {
            bidderNumber: "",
            name: "",
            lastDateModified: "",
            status: ""
        },
        addressAndContact: {
            address1: "",
            address2: "",
            city: "",
            contactPersonName: "",
            phoneNumber: "",
            email: "",
        },
        payment: {
            tinNumber: "",
            paymentTermCode: "",
        },
        serviceCategories: {
            serviceClass: "",
            insurance: false,
            freight: false,
            agency: false,
            storage: false
        }
    });
    Here is the helper function and how it updates the state:
        const handleToggleOnClick = (e, section) => {
        const name = e.currentTarget.getAttribute("data-icon-name");
        setCreateNewBidderFormValues((prevState) => (
            {...prevState, [section]: {...prevState[section], [name]: !prevState.serviceCategories[name]}}
        ))
    }
    So basically, it gets the name of the toggle from the data-icon-name value by using e.currentTarget.getAttribute, and then using setCreateNewBidderFormValues;
    it makes use of the prevState useState hook provides us and then returns an object, spreading the prevState, then using the selection we pass in earlier,
    I use it to select which category I want to update, which is the serviceCategories, and then spread back in the prevState of this category while using the name i got to select which
    key in the serviceCategories I want to update and then set it to always be the opposite vale of the previous state

### Date: [21/08/2023]

### Developer: [Adams Faisal Omokugbo]

### Issue: Figuring out how to integrate devextreme datagrid.

### Solution: So I installed devextreme using npm, below is the main structure to show a table

    import 'devextreme/dist/css/dx.light.css';
    import { Column, DataGrid } from "devextreme-react/data-grid";

    <DataGrid id='dataGrid' dataSource={bidderDashboardRecentBidsData} keyExpr="id">
      <Column dataField="rfqNo" caption="Description" cellRender={({ value }) => <div className="border-b border-greyDark cursor-pointer hover:bg-greyDark">{value}</div>} />
      <Column dataField="description" caption="Description" cellRender={({ value }) => <div className="border-b border-greyDark cursor-pointer">{value}</div>} />
      <Column dataField="expDateAndTime" caption="Description" cellRender={({ value }) => <div className="border-b border-greyDark cursor-pointer">{value}</div>} />
      <Column dataField="status" caption="Description" cellRender={({ value }) => <div className="border-b border-greyDark cursor-pointer">{value}</div>} />
    </DataGrid>

    The Datagrid component is the parent component and recieves dataSorce and keyExpr as props.

    The keyExpr property in the DevExpress DataGrid component is used to specify the unique identifier for each data item in the data source. It helps React and the DataGrid component efficiently manage and update the list of items by providing a stable identifier for each item. This is particularly important when data is added, removed, or updated, as React uses these keys to track changes and perform optimized rendering.

    In most cases, the keyExpr property should correspond to a unique field or property in your data. For example, if your data has an id field that uniquely identifies each item, you would set keyExpr="id".

    Here's why the keyExpr property is important:

        Efficient Rendering: React uses keys to efficiently update the DOM when data changes. It helps React identify which items have changed, been added, or been removed. If keys are not provided or are not unique, React might have to re-render the entire list even if only one item changes, which can lead to performance issues.

        Optimized Updates: When you modify data in the DataGrid (e.g., sorting, filtering, editing), the keyExpr allows the DataGrid to update only the necessary rows instead of re-rendering the entire grid. This improves the overall performance and user experience.

        Stable Order: Keys ensure that the order of items remains stable across renders. Without keys, React could change the order of items, which might lead to unexpected behavior.

    The dataSource is the data source.

### Date: [04/09/2023]

### Developer: [Adams Faisal Omokugbo]

### Issue: Figuring out how to style devextreme datagrid to my taste.

### Solution: So I did some inspecting to find which classes are giving the important style, and gave my own style the important keyword to override the one given by devextreme.

    import 'devextreme/dist/css/dx.light.css';
    import { Column, DataGrid } from "devextreme-react/data-grid";

    const onRowPrepared = (e) => {
        if(e.rowType === 'data') {
            e.rowElement.classList.add("cursor-pointer");
        }
    }
    return <>
    <DataGrid id='dataGrid' dataSource={bidderDashboardRecentBidsData} keyExpr="id" onRowPrepared={onRowPrepared}>
      <Column dataField="rfqNo" caption="Description" cellRender={({ value }) => <div className="">{value}</div>} />
      <Column dataField="description" caption="Description" cellRender={({ value }) => <div className="">{value}</div>} />
      <Column dataField="expDateAndTime" caption="Description" cellRender={({ value }) => <div className="">{value}</div>} />
      <Column dataField="status" caption="Description" cellRender={({ value }) => <div>{value === "pending" ?  <span className="rounded-full px-3 py-1 space-x-2 text-sm bg-yellow-100"><span className="inline-flex items-center"><StatusCirclePendingSVG className="inline" /></span><span>{value}</span></span> : <span className="rounded-full px-3 py-1 space-x-2 text-sm bg-green-200"><span className="inline-flex items-center"><StatusCircleCompletedSVG className="self-center inline" /> </span><span>{value}</span></span>}</div>} />
    </DataGrid>
    </>

    The onRowPrepared is a callback function that allows you to customize and prepare the appearance and behavior of individual rows in the grid based on your own conditions. So i check if the rowType is equal to data so that the class wont be added to the headers, and then i add the tailwind class.

    Below is the style i wrote in the index.css file to override the devextreme default styling:
    .dx-datagrid-rowsview tr {
        border-top: none !important;
        border-bottom-width: 1px !important;
        border-color: #808080 !important;
    }

### Date: [04/09/2023]

### Developer: [Adams Faisal Omokugbo]

### Issue: Error anytime I create a remote repository with a readme and try to push from an existing local repository.

    git push -u origin master

    To https://github.com/Faisal-pixel/Issues-and-Resolutions-Javascript.git
    ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'https://github.com/Faisal-pixel/Issues-and-Resolutions-Javascript.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

### Solution: The above error is copied from my cmd. Every single time I create a new repository on my github and select add Readme.md, I always have this error pushing to the repository from my existing local repository.

    The way I have solved is to first connect the local repository to remote repository <git remote add origin <remote-repository-link>>
    Then run git pull.

Then you most likely will see this message: If you wish to set tracking information for this branch you can do so with:

git branch --set-upstream-to=origin/<branch> master

    The message you're seeing is suggesting that you can set up tracking information for your local master branch to a corresponding branch on the remote repository (typically named origin/master). This tracking information allows Git to understand the relationship between your local branch and the remote branch, making it easier to push and pull changes.

    Here's what the command git branch --set-upstream-to=origin/<branch> master means:

    git branch: This is the Git command to manage branches.
    --set-upstream-to: This option is used to set up tracking information.
    origin/<branch>: This part refers to the remote branch you want to set as the upstream branch. origin is the default name of the remote repository where you fetched or cloned the repository from, and <branch> is the name of the branch on the remote repository.
    master: This is the name of the local branch you want to set up to track the remote branch.

    So, when you run this command, it establishes a connection between your local master branch and the corresponding branch on the remote repository (typically origin/master). This allows you to use commands like git pull and git push without specifying the remote branch explicitly since Git will now know where to fetch and push changes.

After this, I ran [git pull origin master --allow-unrelated-histories]

    Keep in mind that merging unrelated histories should be done with caution, as it can lead to unexpected results, especially if the branches have significant differences in their commit histories.
    Then run git push -u origin master

### Date: [04/09/2023]

### Developer: [Adams Faisal Omokugbo]

### Issue: Using Redux Persist in Redux.

### Solution: Read below

    1) Install redux persist in your app: npm install redux-persist
    2) import { persistStore, persistReducer } from 'redux-persist';
       import storage from 'redux-persist/lib/storage';
    3) set up a configuration object to configure what we want to store.
        <code>
            const persistConfig {
                key: "root"     //specify the key under which the persisted state will be stored in the storage system (such as localStorage or AsyncStorage),
                storage,
                blacklist: ["user"] // list of properties of the reducer, in this rootReducer, that you want to ommit.
            }
        </code>
    4) pass the reducer you want to persist and the persistConfig into the persistReducer
        <code>
            const persistedReducer = persistReducer(persistConfig, rootReducer)
        </code>
    5) create the store and pass the persistedReducer into the createStore.
        <code>
            export const store = createStore(persistedReducer);
            export const persistor = persistStore(store);
        </code>
    6) In the index.js:
        <code>
            import {PersistGate} from redux-persist/integration/react;
            also import the import the persistor and the store.
            <Provider store={store}>
                <PersistGate persistor={persistor}>
                    <App />
                </PersistGate>
            </Provider>
        </code>

    ALSO:
    In a Redux-based application, you typically have different parts of your state that are managed by different reducers or slices. Each of these parts can be considered a separate part of the application. For example, you might have:

    Authentication: This part of the application manages user authentication state, including tokens, user information, and authentication status.

    Shopping Cart: If your application has e-commerce functionality, you might have a part of the state dedicated to managing the contents of the shopping cart.

    User Preferences: This part could store user preferences, such as theme preferences, language settings, or any other user-specific settings.

    Blog Posts: If your application displays blog posts, you might have a part of the state dedicated to managing the list of blog posts and their details.

    User Profile: This could store information about the currently logged-in user's profile.

    Orders History: If your application allows users to view their order history, you might have a part of the state that manages this data.

    Each of these parts of the state can have its own reducer or slice, and Redux Persist can be configured to persist each of these parts separately. This separation ensures that the state for one part doesn't interfere with the state for another part.

    <code>
        import { persistStore, persistReducer } from 'redux-persist';
        import storage from 'redux-persist/lib/storage'; // Choose your storage method
        import { combineReducers, createStore } from 'redux';
        import { authReducer } from './authReducer';
        import { cartReducer } from './cartReducer';
        import { preferencesReducer } from './preferencesReducer';

        const authPersistConfig = {
        key: 'auth', // This will store the authentication state under the key "auth"
        storage,
        };

        const cartPersistConfig = {
        key: 'cart', // This will store the shopping cart state under the key "cart"
        storage,
        };

        const preferencesPersistConfig = {
        key: 'preferences', // This will store the user preferences state under the key "preferences"
        storage,
        };

        const rootReducer = combineReducers({
        auth: persistReducer(authPersistConfig, authReducer),
        cart: persistReducer(cartPersistConfig, cartReducer),
        preferences: persistReducer(preferencesPersistConfig, preferencesReducer),
        // Add more reducers here for other parts of your state
        });

        const persistedReducer = persistReducer(persistConfig, rootReducer);

        // Create the Redux store with the persisted reducer
        const store = createStore(persistedReducer);
        const persistor = persistStore(store);

        export { store, persistor };
    </code>

### Date: [06/09/2023]

### Developer: [Adams Faisal Omokugbo]

---

### Issue: Figuring out how to integrate WebShareApi.

### Solution: Created an object called that stors the text I want to share, in the example below, it is the texToShare object. Then I created an async function that calls the navigator.share() function and catches the error.

    const textToShare = {
        title: 'Zakat Chain',
        text: customMessage,
        url: url
    };

    const handleShare = async () => {
        try {
          await navigator.share(textToShare);
        } catch (error) {
          alert('Error sharing:', error.message);
        }
      };

### The I attached the handle share to a button

### Date: [10/02/2024]

### Developer: [Adams Faisal Omokugbo]

### Issue: How To Redirect Someone To Send an Email From a Client.

### Solution: Used the windows.location.href property. Below is the code:

    window.location.href = `mailto:adamsfaisal2003@gmail.com?subject=${data.subject}&body=Hi, my name is ${data.name}. ${data.message}`; // Anything string can be passed to subject and body.

### Date: [21/05/2024]

### Developer: [Adams Faisal Omokugbo]

### Issue: How To Import an svg file into a react app created with vite.

### Solution: Install vite-plugin-svgr or Import the svg normally and then pass it as the src into the image tag:

    npm install vite-plugin-svgr --save-dev
    - Update vite.config.js: Create or update your vite.config.js file in the root of your project with the following content:
            import { defineConfig } from 'vite';
            import react from '@vitejs/plugin-react';
            import svgr from 'vite-plugin-svgr';

            export default defineConfig({
            plugins: [
                react(),
                svgr(),
            ],
        });
    - Create TypeScript Declaration File: Create a TypeScript declaration file named svg.d.ts in the src directory (or the root directory of your project):
        // src/svg.d.ts
        declare module "*.svg" {
        import * as React from "react";
        export const ReactComponent: React.FunctionComponent<React.SVGProps<SVGSVGElement>>;
        const src: string;
        export default src;
        }
    - Ensure that the include field in your tsconfig.json includes the directory where svg.d.ts is located:
        {
            "compilerOptions": {
                // other options
            },
            "include": ["src", "src/svg.d.ts"]
        }
    - import LogoIcon from "@/assets/images/logo.svg?react";

    OR

    - import LogoIcon from "@/assets/images/logo.svg";
    - <img src={LogoIcon} alt="Logo" />
    - One thing to note is that, if you do not set a defined size for the img tag, the svg will keep on increasing with the content of the parent div.


### Date: [06/06/2024]

### Developer: [Adams Faisal Omokugbo]
