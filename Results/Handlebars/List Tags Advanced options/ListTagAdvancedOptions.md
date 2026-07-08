
# Tags display options #

This sample shows who far we can go today towards the idea that the Tags will be clickable FILTER links.

However, this is not a perfect solution yet. The tags are clickable, but the filter is not working yet as we don't have the infoformation for the filter parameters in the template.

When we apply a filter the URL looks like this:

```
?f=%5B%7B"filterName"%3A"RefinableString116"%2C"values"%3A%5B%7B"name"%3A"1%20year"%2C"value"%3A"or(GP0%7C%2310915784-e3b6-4da8-91bd-2c00195f74e8%2CL0%7C%23010915784-e3b6-4da8-91bd-2c00195f74e8)"%2C"operator"%3A0%2C"disabled"%3Afalse%7D%5D%2C"operator"%3A"and"%7D%5D#

Once we have cleaned up the filter parameters we can see that we need:
-The filterName, here RefinableString116
-The Term label, here 1 year
-The Term Set ID encoded,here GP0%7C%2310915784-e3b6-4da8-91bd-2c00195f74e8 
-The Term ID encoded, here L0%7C%23010915784-e3b6-4da8-91bd-2c00195f74e8
-The operator, here "and"


If we look at that common set of values in the owsmetadatafacetinfo property we can see that some the values are there

```
searchDepartment|search_Department|dZO71pbPg0OD3Yv13A/mfQ==|6snYjlJwHUyk17nBC//qbw==|#2d4f533b-7e7b-47e5-91d9-f4e0b984427b|Legal;

#searchLocation|search_Location|dZO71pbPg0OD3Yv13A/mfQ==|s2SftCJHNkOaXFbDJrNE1A==|#3b89d280-5fcb-4442-ad7c-ad4e3d6061cd|Americas;

#searchTaxonomyMulti|search_TaxonomyMulti|dZO71pbPg0OD3Yv13A/mfQ==|KQDR7IWI/0KIKno+siYXgQ==|#10915784-e3b6-4da8-91bd-2c00195f74e8|1 year;

#searchTaxonomyMulti|search_TaxonomyMulti|dZO71pbPg0OD3Yv13A/mfQ==|KQDR7IWI/0KIKno+siYXgQ==|#c33c64fe-5719-457f-8c29-399f655bb763|3 years;

#searchTaxonomySingle|search_TaxonomySingle|dZO71pbPg0OD3Yv13A/mfQ==|UTUnR9pUOE2EqQq+t+OdPQ==|#219f5678-9007-4e55-959f-280a5a999938|B, after A;#
```
