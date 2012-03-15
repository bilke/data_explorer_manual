Working with the Data Explorer
==============================

Loading data...
---------------

### Native File Formats

OpenGeoSys has a large number of native file formats for storing geometrical data (*.gli), meshes (*.msh), processes (*.pcs), FEM conditions (such as boundary conditions (*.bc)) and material properties (e.g. fluid properties (*.mfp)) and many more.

Not all of them can be loaded into the user interface since not all of them contain data that can be visualised. Things are additionally made difficult as the file standards for the programm are currently changed to XML files[^1]. For this reason there exists – at least at the time of this writing – two file standards for certain kinds of objects. Hopefully, this unfortunate situation is resolved soon.

You can open native OGS files by clicking *File / Open*. Supported native OpenGeoSys formats are:

-   Geometrical data files (*.gli)

    Containing geometrical data such as points, polylines and surfaces (in this file format surfaces are defined by closed polylines and are therefore always describing planes).

-   Geometrical XML data files (*.gml)

    Containing geometrical data such as points, polylines and surfaces (in this file format surfaces are triangulated and can therefore describe any kind of surface).

-   Meshes (*.msh)

    Domain discretisation in 2D or 3D. Each mesh contains a set of nodes (points) as well as a set of elements defined by a subset of these nodes. A mesh may contain different kinds of elements. OGS supports the following element types: lines, triangles, quadrilaterals, tetrahedra, hexahedra, pyramids, prisms.\
    Meshes may be classified into structured or unstructured meshes. Note, that OpenGeoSys treats all meshes as unstructured.

-   Observation sites (*.stn)

    XML file format containing information for observation stations as well as boreholes. The files also describe points in space but with additional information (such as stratigraphic information for boreholes). Also, this data is visualised in a different way.

-   OpenGeoSys roject files (*.gsp)

    XML file format for OpenGeoSys projects. These files contain files related to a given project. All of these files will be loaded upon loading the *.gsp file. This is especially useful for projects consisting of a large number of input files.

Additionally, right-clicking on any geometry in the data view will give you the option Add FEM Condition... which allows you to load FEM condition files in ASCII (*.bc, *.ic, *.st) or XML-format (*.cnd). These files describe initial and boundary conditions as well as source terms that are associated with pre-defined geometrical objects (points, polylines, surfaces) with additional information important for a subsequent simulation. FEM conditions can also be loaded via *File / Open*.... Upon loading it is then checked if a corresponding geometry for these conditions is available. For XML-files this geometry is saved in the cnd-file, for old file types it is simply assumed that the geometry name will be the same as the name of the condition-file. If no geometry of this name is found, the data will not be loaded.

As an alternative, you may add source terms of the process distribution `DIRECT` directly on meshes nodes (again, via right-clicking on a mesh) and the conditions will then be displayed on the respective mesh nodes defined in the input file. For the visualisation, the correct scalar values in the render window will currently only be displayed for process distribution types `CONSTANT`, `LINEAR` and `DIRECT`.[^2]

### Import File Formats

![Supported file formats for import and export of data][interfaces]

A number of non-native file formats can also be loaded and visualised in the Data Explorer. To import these files click on File Import Files and select the appropriate entry.

Depending of which file format you want to load, quality of the interface varies depending on a number of facts such as if the file format is an open standard or if there were enough input files to test the interface when it was implemented.

Currently the following file formats are supported:

-   ESRI **shape** files (*.shp)\
    Vector files specifying points, polylines and polygons. The interface for shape files is thoroughly tested and there should be no problems whatsoever. However, note that a lot shape files come with a database file containing additional information (*.dbf) which has no standardised table structure and therefore is not analysed or imported by the data explorer.

-   Aquaveo **GMS** files (*.txt, *.3dm)\
    Text or mesh files specifying boreholes (*.txt) or layered meshes build from tetrahedrons and prisms. This interface has been tested with number of cases and should work fine.

-   **GMSH** files (*.msh)\
    Mesh files containing unstructured grids. As an exception these files are *not* loaded via the import menu but you have to load them directly via *File / Open*. The interface should work perfectly however.

-   **GoCAD** files (*.ts)\
    This interface is in an unfinished state. However, there exists a programming interface to write OGS-files directly from GoCAD. Please talk to the development team if you need to load data into OGS.

-   **NetCDF** files (*.nc, *.cdf)\
    A machine-independent format that contains all kinds array-oriented scientific data. This interface currently only works under certain circumstances and you should probably talk to the development team if you want to load netCDF files into OGS. The result will be a mesh with data values from the netCDF files assigned on the elements. (The workflow is similar to creating meshes from images/rasters, see section [meshraster])

-   **FEFLOW** files\
    Allows the import FEFLOW problem ASCII files (*.fem). This interface imports only geometry, e.g. polygons, and meshes.

-   **Petrel** files\
    This interface is in an unfinished state. Please talk to the development team if you need to load data into OGS.

-   **Raster** files (*.asc, *.bmp, *.jpg, *.png, *.tif)\
    Image data files that may contain satellite images, etc. Ascii raster files (*.asc) and GeoTIFF (*.tif) files contain geo-referenced data, while common image files (*.bmp, *.jpg, *.png) do not. This interface should work perfectly although it can be quite slow for large raster files.

-   **TetGen** files\
    Allows the import files created with the TetGen mesh generator. This interface currently only reads the node- and tetrahedra-files created by the software (i.e. *.node and *.ele).

-   Visual Toolkit (**VTK**) files (*.vti, *.vtk, *.vtp, *.vtr, *.vts, *.vtu)\
    Files containing data for graphic objects, ranging from image files (*.vti) to structured (*.vts) or unstructured meshes (*.vtu). All data sets may include additional data in the form of scalar arrays. This interface should work perfectly.

### Removal of Data

You can remove all data loaded into OpenGeoSys by right-clicking the data set in its respective data view and selected Remove data. Specifically you can also remove only polylines or surfaces only from a geometry. The only exception to this rule are geometric points which can only be removed if both surfaces and polylines are already deleted as both kinds of objects are dependent on points. Regardless of the previous remarks you can also remove geometries as whole.

Data Visualisation
------------------

[datavisualisation]

Technically, data (or a representation thereof) loaded into the programme is displayed at three different locations (see figure [fig:gui]):

### Data Views

There are four different DataView-Tabs in the programme where the data is visible in the form of lists. Which of the tabs is used depends on the data. There is one tab for geometrical information, one for meshes, one for stations (i.e. observation sites) and one for modelling information (i.e. processes, boundary conditions, etc.)

The DataView for geometrical information contains a list of geometries. Each geometry-item has up to three children titled “Points”, “Polylines” and “Surfaces” containing the information about the respective geometrical objects. For example, the item ‘Points’ contains a list of points and for each item (i.e. each point) its index, the coordinates and (if existing) the name of the object can be displayed. Each geometry *needs* to contain a list of points. Other geometrical objects (i.e. polylines or surfaces) are optional. Upon selecting a geometrical object in the DataView, the object will also be highlighted in the render window. For better visibility a point will be marked by a small ball, a line by a tube.

Note, that the mesh-tab is subdivided into a view of the meshes loaded into the programme (and the elements contained within each mesh) and a second window called “Element Properties”. If you pick any element from the render window, all available information concerning that element (such as nodes, volume, element type, etc.) will be displayed here. For more information on how to pick mesh elements, see the next section.

### Render Window

![Examples for inconsistencies within the data. The left image show inconsistencies between two meshes][error1]

![The right images show a number of boreholes, one of which has a wrong offset.][error2]

Examples for inconsistencies within the data. The left image show inconsistencies between two meshes. The right images show a number of boreholes, one of which has a wrong offset. [fig:error]

The render window is the part of the GUI where all the data is visualised in a user-controlled 3D scene. The process of “drawing” an object in the render window is technically called the *rendering* of the object and will be referred to as such in the following. The render window is where you can actually see your data as well as the effects of all the changes you do somewhere else in the programm or in the input files.

One of the big advantages of the Data Explorer is in the fact that this visual inspection of the data also allows you to assess your data and to find inconsistencies and errors. Figure [fig:error] gives examples of such inconsistencies.

You can manipulate the 3d view by using the mouse. Holding the left mouse button and moving the mouse will rotate everything around the focal point of the scene (by default this is the center point of your data). By holding the middle mouse button you can pan the view which means you translate left / right and up / down. By holding the right mouse button or moving the mouse wheel you can zoom in and out.

There is alternative mouse button functionality assignment which is activated by holding the Spacebar. When activated, left clicks pick a cell of the actually selected visualisation pipeline object. This allows to mark a certain cell of the visualisation, such as the selection of a mesh element for displaying information related to that element. On right clicking the picked position is set as the focal point of the camera. This is useful for a better examination of an interesting region of the data so with focal point set to that region you can easily rotate and zoom around.

You can deselect the cell by holding Spacebar and clicking somewhere into the background of the render window.

Note, that you can make screenshots of the content of the render window at any time. Simply press the Screenshot-button located above the render window and a small dialog will open that allows you to select the scaling factor of the image. This is useful for creating large image (e.g. for posters) without artefacts.

### Visualization Pipeline

The visualisation pipeline looks very similar to the data views described above. However, the items displayed in this tab are a list of the graphical objects displayed (rendered) in the render window. For further reference these will be called *visualisation items*. Each visualisation item has a checkbox beside its name that determines if the object is displayed in the render window or not. You can check or uncheck this box at any time, no data is lost if you uncheck it.

The relationship between data views, visualisation pipeline and render window is not completely straightforward without knowing the internal data structures of the programme but the basic concept is this:

For each data set loaded into the programme one or more visualisation items are created and then displayed in the render window. In most cases the relationship is not that difficult to understand: If a mesh is loaded, a new visualisation item is created and displayed in the render window. If a list of observation sites is loaded, again one visualisation item is created that represents the graphical object displayed in the render window. While you can expand that list of stations in the data view and see information about each station contained in the list, the visualisation item cannot be expanded. It is a substitute for the visualisation of said list in the render window. In the same vain, if a geometry is loaded into the programme up to *three* visualisation objects are created: one for the list of points, one for the list of polylines and one for the list of surfaces (depending on the existence of polylines and surfaces).

Section [filters] explains how you can employ the visualisation pipeline to apply filters the visualisation items. These filters allow changes of the way each object is visualised and they are quite handy to show certain aspects of the data.

Writing Data...
---------------

### Native Files

You can save geometries, observation sites and meshes by right-clicking them in the Data View and selecting Save data.... You can also save all loaded data files by saving an OpenGeoSys project. Select *File / Save as*. and specify a project name. This option will save all geometries in *.gml files, all mesh meshes in *.msh files and all observation sites in *.stn files. Additionally it will create a project file (*.gsp). When loading the gsp-file later on it will also load the respective geometries and observation sites again. (Note: Other file formats – e.g. for boundary conditions or processes – will be added in the future)

### Export to Other Formats

Any geometric data loaded into OpenGeoSys can be exported into a GMSH *.geo file via FileSave as... and selecting “GMSH gemetry files” as the output format. Note, that this will merge all geometries loaded into the programme and that it will also save station-data as points (i.e. you lose any additional information associated with these stations). Before saving, the geometric information will be inserted into a quad tree structure to analyse the data and insert additional points (so-called ‘Steiner Points’) at locations where not enough information for generation of a suitable mesh exists. This ensures the creation of an adequate result when meshing the geo-file within GMSH.[^3]

You can also export data from OpenGeoSys into a number of graphics formats. This can be done for selected graphical objects by right-clicking the respective object in the visualisation pipeline and than select the desired format (VTK or OpenSG). Details on this can be found in section [specvisoptions].

You can also export the complete scene into the graphic formats VTK, OpenSG or VRML format. To do this select FileExportFormat.

[^1]: http://www.w3.org/XML/

[^2]: For an overview over what process distribution types are, which types are supported by the system and how they influence a subsequent simulation please refer to technical documentation of OpenGeoSys.

[^3]: Note that this allows you to export any combination of data into GMSH format for subsequent meshing. The functionality described in section [meshcreation] for meshing of data within OpenGeoSys requires a “bounding polygon” that serves as outer boundary of all the data contained within the mesh.

[interfaces]: pics/interfaces.png
[error1]:     pics/error1.png
[error2]:     pics/error2.png