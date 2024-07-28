
# Layouts for Quick Links Look-a-likes #

these layout are part of the answer to a [request](https://github.com/microsoft-search/pnp-modern-search/discussions/3876#discussioncomment-10135520) in the PnP Modern Search repository 

The basic idea is to create a layout that looks like the Quick Links web part in SharePoint.

**Step 1** is to create the Content type and the columns that are needed for the "Quick Link". I suggest creating this content type in the Content type gallery and then adding it to the list or library where you want to use it.

![Content type](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_ContentType.png)

**Step 2** is to create a few items in the list or library with the content type. This will trigger the search indexer and create the Managed Properties needed for the Handlebars template.
![Content in list](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_list_content.png)

The values in the QuickLinksInPnPModernSearchIconName column are the names of the icons in the [Office UI Fabric](https://uifabricicons.azurewebsites.net/) library. 
The values in the QuickLinksInPnPModernSearchLink column are the links to the pages or documents.


**Step 3** is to create a Document library and add the HTML layouts from this folder.

**Step 4** is to create the page and add the Results web part to the page.

**Step 5** is to configure the web part to use the layout and the list or library. Add the 4 managed properties to Selected Properties. They are all named "QuickLink"something. Update the layout slot as this:
![Layout Slots](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_LayoutSlots.png)

**Step 6** if you want the content to be sorted by a specific Property, you will need to add a sort order to the query. This is done in the Sort settings in the web part.

**Step 7** is to change the layout to one of the layouts in this folder.
Change to "Custom" and add the path to the layout in the Document library.
![custom layout](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_Use_custom_layout.png)



## Simple layout ##
The List Tags Simple option looks like this in if you want the tags to be displayed in a simple one column list:

![Simple](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_simple_layout.png)


## Extented layout ##
![Extended](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_extended_layout.png)


## Extented layout with User ##
![Extended with User](/Results/Handlebars/Quick%20Links%20lookalikes/assets/QuickLinksInPnPModernSearch_extended_layout_with_user.png)


In each of the layouts, you can change the CSS to fit your needs. 

