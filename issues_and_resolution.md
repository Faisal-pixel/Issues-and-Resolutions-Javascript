# ** TABLE OF CONTENT **

    1. Figuring out how to add the toggle button in the Create New Bidder form under the service categories, and also updating it in the state.
    2. Figuring out how to integrate devextreme datagrid.
    4. Figuring out how to style devextreme datagrid to my taste.

### Bug: Figuring out how to add the toggle button in the Create New Bidder form under the service categories, and also updating it in the state.

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

### Bug: Figuring out how to integrate devextreme datagrid.

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

### Bug: Figuring out how to style devextreme datagrid to my taste.

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
