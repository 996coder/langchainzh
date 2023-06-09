


 Google Drive
 [#](#google-drive "Permalink to this headline")
===============================================================



 This notebook covers how to load documents from Google Drive. Currently, only Google Docs are supported.
 




 Prerequisites
 [#](#prerequisites "Permalink to this headline")
-----------------------------------------------------------------


1. Create a Google Cloud project or use an existing project
2. Enable the
 [Google Drive API](https://console.cloud.google.com/flows/enableapi?apiid=drive.googleapis.com)
3. [Authorize credentials for desktop app](https://developers.google.com/drive/api/quickstart/python#authorize_credentials_for_a_desktop_application)
4. `pip
 

 install
 

 --upgrade
 

 google-api-python-client
 

 google-auth-httplib2
 

 google-auth-oauthlib`





 🧑 Instructions for ingesting your Google Docs data
 [#](#instructions-for-ingesting-your-google-docs-data "Permalink to this headline")
-----------------------------------------------------------------------------------------------------------------------------------------



 By default, the
 `GoogleDriveLoader`
 expects the
 `credentials.json`
 file to be
 `~/.credentials/credentials.json`
 , but this is configurable using the
 `credentials_path`
 keyword argument. Same thing with
 `token.json`
 -
 `token_path`
 . Note that
 `token.json`
 will be created automatically the first time you use the loader.
 



`GoogleDriveLoader`
 can load from a list of Google Docs document ids or a folder id. You can obtain your folder and document id from the URL:
 


* Folder: https://drive.google.com/drive/u/0/folders/1yucgL9WGgWZdM1TOuKkeghlPizuzMYb5 -> folder id is
 `"1yucgL9WGgWZdM1TOuKkeghlPizuzMYb5"`
* Document: https://docs.google.com/document/d/1bfaMQ18_i56204VaQDVeAFpqEijJTgvurupdEDiaUQw/edit -> document id is
 `"1bfaMQ18_i56204VaQDVeAFpqEijJTgvurupdEDiaUQw"`







```
from langchain.document_loaders import GoogleDriveLoader

```










```
loader = GoogleDriveLoader(
    folder_id="1yucgL9WGgWZdM1TOuKkeghlPizuzMYb5",
    # Optional: configure whether to recursively fetch files from subfolders. Defaults to False.
    recursive=False
)

```










```
docs = loader.load()

```








