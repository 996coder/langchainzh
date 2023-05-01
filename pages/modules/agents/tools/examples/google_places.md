


 Google Places
 [#](#google-places "Permalink to this headline")
=================================================================



 This notebook goes through how to use Google Places API
 







```
#!pip install googlemaps

```










```
import os
os.environ["GPLACES_API_KEY"] = ""

```










```
from langchain.tools import GooglePlacesTool

```










```
places = GooglePlacesTool()

```










```
places.run("al fornos")

```








```
"1. Delfina Restaurant\nAddress: 3621 18th St, San Francisco, CA 94110, USA\nPhone: (415) 552-4055\nWebsite: https://www.delfinasf.com/\n\n\n2. Piccolo Forno\nAddress: 725 Columbus Ave, San Francisco, CA 94133, USA\nPhone: (415) 757-0087\nWebsite: https://piccolo-forno-sf.com/\n\n\n3. L'Osteria del Forno\nAddress: 519 Columbus Ave, San Francisco, CA 94133, USA\nPhone: (415) 982-1124\nWebsite: Unknown\n\n\n4. Il Fornaio\nAddress: 1265 Battery St, San Francisco, CA 94111, USA\nPhone: (415) 986-0100\nWebsite: https://www.ilfornaio.com/\n\n"

```







