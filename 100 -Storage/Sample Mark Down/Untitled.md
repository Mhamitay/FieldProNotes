>[!info] This is an info box.


This will generate a table of notes from the specified folder.

### 5. **Block References**
Reference specific blocks of text within your notes:

>[!note] This is a note
>

>[!tip] Here’s a helpful tip!

>[!warning] Be cautious with this!

>[!danger] This is a danger!

>[!info] This is an info box.

> [!example] Notes  

>[[Note Name]]

>[[Note Name#Section Heading]]

![Alt text](image-url.jpg)
[[path-to-file.pdf]]
### 5. **Block References**
Reference specific blocks of text within your notes:

[[My Note#^block-id]]

`inline code`

```C#
var dashboards = GetDashboardsInfoByPath(path);
if(dashboards.Select(x => x.ID).Contains(dashboardID) == false)
{
	dashboardID = dashboards.Where(x => x.Default == true).FirstOrDefault()?.ID;
	if (string.IsNullOrEmpty(dashboardID))
	{
		throw new Exception("You do not have permission to view this dashboard");
	}
}
```


``` dataview
table date, status
from "project-folder"
where status = "active"
```