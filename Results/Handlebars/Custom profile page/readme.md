# Custom Profile Page #

Use the PnP Search Results web part to display information from the SharePoint user profile to create a custom profile page.

![Screenshot of custom profile page layout](assets/custom-profile-page.png)

For step-by-steps on how to set up a custom profile page layout, see this blog post: [https://corriehaffly.wordpress.com/2025/01/19/creating-a-custom-sharepoint-profile-page-using-the-pnp-modern-search-solution/](https://corriehaffly.wordpress.com/2025/01/19/creating-a-custom-sharepoint-profile-page-using-the-pnp-modern-search-solution/)

The blog post provides information on displaying basic OOTB SharePoint user profile properties, custom user profile properties, and information from elsewhere in SharePoint related to the user. 

- **people-searchresults.html** is a basic people search results template based on configurations with the MGT toolkit and hover card options turned on, but the people cards have had a link added to go to a profile.aspx page, passing through the email as a query parameter.
- **toprow.html** is meant to be used in a one-column section with soft background color. It shows a larger version of the profile picture, the user's name, title, and department. Be sure to select the JobTitle and Department properties and map to layout slots.
- **toprow-customprop.html** is a variation of toprow.html which includes bringing in a custom SharePoint user profile property for "AudioPronunciation." You will have to create the custom user profile property, map it to a custom managed property, then select the managed property and map it to a layout slot in the search results web part.
- **left-contactinfo.html** displays the user's email, phone number(s), and office location and demonstrates custom styling for attractive labels.
- **right-bio.html** brings in data from a custom list which is related to the current displayed user. The search result query template must include `PersonColumnManagedProperty: {QueryString.q}` to properly filter results by the user, where PersonColumnManagedProperty is the managed property mapped to your list's person field.
