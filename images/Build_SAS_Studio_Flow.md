![Global Enablement & Learning](https://gelgitlab.race.sas.com/GEL/utilities/writing-content-in-markdown/-/raw/master/img/gel_banner_logo_tech-partners.jpg)

# Build a SAS Studio Flow
* [Exercise Description](#exercise-description)
* [SAS Viya Logon Info](#sas-viya-logon-info)
* [Create a SAS Studio Flow](#create-a-sas-studio-flow)
  * [Join the Table and Imported File](#join-the-table-and-imported-file)
  * [Run and Preview Results](#run-and-preview-results)
  * [Add Output Table Step to Flow](#add-output-table-step-to-flow)
* [Exercise Completed](#exercise-completed)

<br>

## Exercise Description
SAS Studio flows provide a visual, drag-and-drop interface within SAS Studio that allows users to build, manage and execute complex analytics workflows without writing code. In this hands-on workshop, you'll learn to build a flow using steps to access, prepare and analyze your data.

You will create a SAS Studio flow that uses SAS data and an imported text file.  You will join the data sets and write the output to a SAS Library.

<br>
<br>

## SAS Viya Logon Info
Use the *Google Chrome* browser and select the **SAS Drive** bookmark.

ID: **student**
Password: **Metadata0**

Select **No** when prompted about accepting *Admin* privileges.

<br>
<br>

## Create a SAS Studio Flow
1. Select ![Viya Menu Selector](images/HamburgerMenu.png) **&#10132; Develop Code and Flows** to open *SAS Studio*.
1. Select **New &#10132; Flow**.
1. Select ![Steps Pane](images/Steps.png) to view the **Steps** pane.

     ![Create Flow](images/CreateFlow.png)

1. Double-click the **Table** step in the *Data (Input and Output)* section of the *Steps* pane to add it to the flow canvas.
1. In the **Table Properties** section select the following:
   - Library:  **SASHELP**
   - Table name: **CARS**

     ![CARS Table Properties](images/CARSTableProperties.png)

1. Select **Preview Data** to view a subset of the rows from the table.
1. Select **Node** to name the step in the flow.
1. Add *Description* to **CARS Table from SASHELP library**.

     ![CARS Table Node](images/CARSTable.png)

1. Select ![Explorer pane](images/Explorer.png) to open the **Explorer** pane.
1. Navigate to **Files &#10132; Home &#10132; workshop &#10132; SIWSTF**.
1. Drag the file **Car_Make_Info.csv** on to the flow canvas.

     ![Car Make Info file](images/CarMakeInfoFile.png)

1. Select ![Add Step](images/Add.png) **&#10132; Import** to add the *Import* step to the flow canvas.
1. Use the mouse to connect the the *Car_Make_Info* file step to *Import* step.
1. Select ![Rearrange Flow](images/Rearrange.png) to rearrange the steps in the flow.

     ![Import Step](images/ImportFile.png)

1. In the *Options* section for the *Import* step, select **Analyze** to generate the columns for the connected file.

     ![Analyze File](images/Analyze.png)

1. Select ![Save](images/Save.png) to save the flow.
1. Navigate to **SAS Content &#10132; Public**.
1. Enter **Car_Make** for the *name* and click **Save**.

   ![Save Flow](images/CarMakeSave1.png)

<br>

### Join the Table and Imported File
1. Add the **Query** step in the *Transform Data* section of the *Steps* pane to the flow canvas and connect it to both the **CARS** *Table* step and the **Import** step.  This creates two input ports for the *Query* step.

     > &#9755; You can drop the **Query** node on the right-hand side of the **CARS** *Table* node in flow canvas to auto-connect them and then manually connect the *Import* step to it.

2. **Rearrange** the flow and **save** it.

     ![](images/AddQueryStep.png)


1. Select the following columns on the *Select* tab:

     | Table | Source | Name | Label |
     |  :---  |  :---:  |  :---:  |  :---:  |
     | t2 | PARENT_COMPANY | **Parent_Company** | **Parent Company** |
     | t1 | Make | Make | **Car Make** |
     | t1 | Model | Model | **Car Model** |
     | t1 | Type | Type | **Car Type** |
     | t1 | Origin | Origin | **Car Origin** |
     | t1 | MSRP | MSRP | **MSRP** |
     | t1 | Invoice | Invoice | **Invoice** |
     | t2 | WEBSITE | **Website** | **Main Website** |

     > &#9998; This assumes that **t1** is the *CARS Table* step and **t2** is the *Import* step.

     ![](images/QuerySelect.png)

1. Click ![](images/AddCalcColumn.png) to add a calculated column.

1. Enter the following for the calculation:

     **t1.MSRP - t1.Invoice**

     > &#9998; This assumes that **t1** is the *CARS Table* step.

1. Enter the following for the properties:

     - Column name: **Diff**
     - Column label: **Difference**
     - Type: **Numeric**

     ![](images/DiffCalcColumn.png)

1. Click **Save** to save the calculated column.

     ![](images/QuerySelectCalc/.png)

1. On the *Join* tab, confirm that the join condition is **t1.Make=t2.MAKE**.

     ![](images/QueryJoin.png)

     > &#9998; This join condition is made automatically since the columns have the same name.

1. Select the following columns on the *Sort* tab:

     | Table | Source | Sort |
     |  :---  |  :---:  |  :---:  |
     | t2 | PARENT_COMPANY | Ascending |
     | t1 | Make | Ascending |
     | t1 | Model | Ascending |

     ![](images/QuerySort.png)

1. On the *Output Options* tab, leave the selection for *Generate code using* as **PROC SQL**.

     ![](images/QueryOutput.png)

1. **Save** the changes to the flow.

<br>


### Run and Preview Results
1. **Run** the flow.

1. Select the **Output port** of the *Query* step and **preview** its results.

     ![](images/QueryRunOutput1.png)

1. On the *Select* tab, of the *Query* node, select ![Change Format](images/ChangeFormat.png) in the *Format* column for the **Diff** calculated column.

     ![](images/SelectChangeFormat.png)

1. Set the format to **DOLLAR8** and click **OK**.

     ![](images/ChangeFormatDiff.png)

     ![](images/DiffDollar.png)

1. **Save** the changes to the flow.
1. **Run** the flow and select the **Output port** of the *Query* step and **preview** its results.

     ![](images/QueryRunOutput2.png)


<br>

### Add Output Table Step to Flow

1. Add a **Table** step from the *Data (Input and Output)* section to the flow and connect it to the *Output port* of the **Query** step.

1. In the **Table Properties** section select the following:

   - Library:  **WORK**
   - Table name: **CARS_INFO**

     > &#9998; This creates an output table for the flow.  You will need to type the new table name.

     ![](images/AddTableFlow.png)

1. **Save** and **Run** the flow.
1. View the **Generated Code** and the **Submitted Code and Results** tabs.

     ![](images/RunFinalFlow.png)

1. Select ![Libraries](images/LibrariesPane.png) to view the Libraries pane.
1. View the **CARS_INFO** table in the **WORK** SAS library.

     ![](images/CARS_INFOTable.png)

1. **Close** the *Table* and the *Flow* file.

<br>
<br>

## Exercise Completed

**YOU HAVE COMPLETED THE EXERCISE ON BUILDING A SAS STUDIO FLOW!**

For additional information on SAS Studio Flows, please refer to its [documentation](https://go.documentation.sas.com/doc/en/sasstudiocdc/default/webeditorcdc/webeditorflows/titlepage.htm).

**THANKS FOR ATTENDING THIS WORKSHOP!**