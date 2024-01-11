- `site:` Include only results on a given hostname
- `intitle:` Filters according to the title of a page
- `inurl:` Similar to intitle but works on the URL of a resource
- `filetype:` Filters by using the file extension of a resource
- `AND`, `OR`, `|` Use logical operators to combine your expressions
- `-` Filter out a keyword or a command's result

Example: `-inurl:(htm|html|php|asp|jsp) intitle:"index of" "last modified" "parent directory" txt OR doc OR pdf`