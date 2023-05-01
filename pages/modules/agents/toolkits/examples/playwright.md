


 PlayWright Browser Toolkit
 [#](#playwright-browser-toolkit "Permalink to this headline")
===========================================================================================



 This toolkit is used to interact with the browser. While other tools (like the Requests tools) are fine for static sites, Browser toolkits let your agent navigate the web and interact with dynamically rendered sites. Some tools bundled within the Browser toolkit include:
 


* NavigateTool (navigate_browser) - navigate to a URL
* NavigateBackTool (previous_page) - wait for an element to appear
* ClickTool (click_element) - click on an element (specified by selector)
* ExtractTextTool (extract_text) - use beautiful soup to extract text from the current web page
* ExtractHyperlinksTool (extract_hyperlinks) - use beautiful soup to extract hyperlinks from the current web page
* GetElementsTool (get_elements) - select elements by CSS selector
* CurrentPageTool (current_page) - get the current page URL







```
# !pip install playwright > /dev/null
# !pip install lxml

# If this is your first time using playwright, you'll have to install a browser executable.
# Running `playwright install` by default installs a chromium browser executable.
# playwright install

```










```
from langchain.agents.agent_toolkits import PlayWrightBrowserToolkit
from langchain.tools.playwright.utils import (
    create_async_playwright_browser,
    create_sync_playwright_browser,# A synchronous browser is available, though it isn't compatible with jupyter.
)

```










```
# This import is required only for jupyter notebooks, since they have their own eventloop
import nest_asyncio
nest_asyncio.apply()

```







 Instantiating a Browser Toolkit
 [#](#instantiating-a-browser-toolkit "Permalink to this headline")
-----------------------------------------------------------------------------------------------------



 It’s always recommended to instantiate using the
 `from_browser`
 method so that the
 







```
async_browser = create_async_playwright_browser()
toolkit = PlayWrightBrowserToolkit.from_browser(async_browser=async_browser)
tools = toolkit.get_tools()
tools

```








```
[ClickTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='click_element', description='Click on an element with the given CSS selector', args_schema=<class 'langchain.tools.playwright.click.ClickToolInput'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 NavigateTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='navigate_browser', description='Navigate a browser to the specified URL', args_schema=<class 'langchain.tools.playwright.navigate.NavigateToolInput'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 NavigateBackTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='previous_webpage', description='Navigate back to the previous page in the browser history', args_schema=<class 'pydantic.main.BaseModel'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 ExtractTextTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='extract_text', description='Extract all the text on the current webpage', args_schema=<class 'pydantic.main.BaseModel'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 ExtractHyperlinksTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='extract_hyperlinks', description='Extract all hyperlinks on the current webpage', args_schema=<class 'langchain.tools.playwright.extract_hyperlinks.ExtractHyperlinksToolInput'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 GetElementsTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='get_elements', description='Retrieve elements in the current web page matching the given CSS selector', args_schema=<class 'langchain.tools.playwright.get_elements.GetElementsToolInput'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>),
 CurrentWebPageTool(sync_browser=None, async_browser=<Browser type=<BrowserType name=chromium executable_path=/Users/wfh/Library/Caches/ms-playwright/chromium-1055/chrome-mac/Chromium.app/Contents/MacOS/Chromium> version=112.0.5615.29>, name='current_webpage', description='Returns the URL of the current page', args_schema=<class 'pydantic.main.BaseModel'>, return_direct=False, verbose=False, callback_manager=<langchain.callbacks.shared.SharedCallbackManager object at 0x10e104290>)]

```










```
tools_by_name = {tool.name: tool for tool in tools}
navigate_tool = tools_by_name["navigate_browser"]
get_elements_tool = tools_by_name["get_elements"]

```










```
await navigate_tool.arun({"url": "https://web.archive.org/web/20230428131116/https://www.cnn.com/world"})

```








```
'Navigating to https://web.archive.org/web/20230428131116/https://www.cnn.com/world returned status code 200'

```










```
# The browser is shared across tools, so the agent can interact in a stateful manner
await get_elements_tool.arun({"selector": ".container__headline", "attributes": ["innerText"]})

```








```
'[{"innerText": "These Ukrainian veterinarians are risking their lives to care for dogs and cats in the war zone"}, {"innerText": "Life in the ocean\\u2019s \\u2018twilight zone\\u2019 could disappear due to the climate crisis"}, {"innerText": "Clashes renew in West Darfur as food and water shortages worsen in Sudan violence"}, {"innerText": "Thai policeman\\u2019s wife investigated over alleged murder and a dozen other poison cases"}, {"innerText": "American teacher escaped Sudan on French evacuation plane, with no help offered back home"}, {"innerText": "Dubai\\u2019s emerging hip-hop scene is finding its voice"}, {"innerText": "How an underwater film inspired a marine protected area off Kenya\\u2019s coast"}, {"innerText": "The Iranian drones deployed by Russia in Ukraine are powered by stolen Western technology, research reveals"}, {"innerText": "India says border violations erode \\u2018entire basis\\u2019 of ties with China"}, {"innerText": "Australian police sift through 3,000 tons of trash for missing woman\\u2019s remains"}, {"innerText": "As US and Philippine defense ties grow, China warns over Taiwan tensions"}, {"innerText": "Don McLean offers duet with South Korean president who sang \\u2018American Pie\\u2019 to Biden"}, {"innerText": "Almost two-thirds of elephant habitat lost across Asia, study finds"}, {"innerText": "\\u2018We don\\u2019t sleep \\u2026 I would call it fainting\\u2019: Working as a doctor in Sudan\\u2019s crisis"}, {"innerText": "Kenya arrests second pastor to face criminal charges \\u2018related to mass killing of his followers\\u2019"}, {"innerText": "Russia launches deadly wave of strikes across Ukraine"}, {"innerText": "Woman forced to leave her forever home or \\u2018walk to your death\\u2019 she says"}, {"innerText": "U.S. House Speaker Kevin McCarthy weighs in on Disney-DeSantis feud"}, {"innerText": "Two sides agree to extend Sudan ceasefire"}, {"innerText": "Spanish Leopard 2 tanks are on their way to Ukraine, defense minister confirms"}, {"innerText": "Flamb\\u00e9ed pizza thought to have sparked deadly Madrid restaurant fire"}, {"innerText": "Another bomb found in Belgorod just days after Russia accidentally struck the city"}, {"innerText": "A Black teen\\u2019s murder sparked a crisis over racism in British policing. Thirty years on, little has changed"}, {"innerText": "Belgium destroys shipment of American beer after taking issue with \\u2018Champagne of Beer\\u2019 slogan"}, {"innerText": "UK Prime Minister Rishi Sunak rocked by resignation of top ally Raab over bullying allegations"}, {"innerText": "Iran\\u2019s Navy seizes Marshall Islands-flagged ship"}, {"innerText": "A divided Israel stands at a perilous crossroads on its 75th birthday"}, {"innerText": "Palestinian reporter breaks barriers by reporting in Hebrew on Israeli TV"}, {"innerText": "One-fifth of water pollution comes from textile dyes. But a shellfish-inspired solution could clean it up"}, {"innerText": "\\u2018People sacrificed their lives for just\\u00a010 dollars\\u2019: At least 78 killed in Yemen crowd surge"}, {"innerText": "Israeli police say two men shot near Jewish tomb in Jerusalem in suspected \\u2018terror attack\\u2019"}, {"innerText": "King Charles III\\u2019s coronation: Who\\u2019s performing at the ceremony"}, {"innerText": "The week in 33 photos"}, {"innerText": "Hong Kong\\u2019s endangered turtles"}, {"innerText": "In pictures: Britain\\u2019s Queen Camilla"}, {"innerText": "Catastrophic drought that\\u2019s pushed millions into crisis made 100 times more likely by climate change, analysis finds"}, {"innerText": "For years, a UK mining giant was untouchable in Zambia for pollution until a former miner\\u2019s son took them on"}, {"innerText": "Former Sudanese minister Ahmed Haroun wanted on war crimes charges freed from Khartoum prison"}, {"innerText": "WHO warns of \\u2018biological risk\\u2019 after Sudan fighters seize lab, as violence mars US-brokered ceasefire"}, {"innerText": "How Colombia\\u2019s Petro, a former leftwing guerrilla, found his opening in Washington"}, {"innerText": "Bolsonaro accidentally created Facebook post questioning Brazil election results, say his attorneys"}, {"innerText": "Crowd kills over a dozen suspected gang members in Haiti"}, {"innerText": "Thousands of tequila bottles containing liquid meth seized"}, {"innerText": "Why send a US stealth submarine to South Korea \\u2013 and tell the world about it?"}, {"innerText": "Fukushima\\u2019s fishing industry survived a nuclear disaster. 12 years on, it fears Tokyo\\u2019s next move may finish it off"}, {"innerText": "Singapore executes man for trafficking two pounds of cannabis"}, {"innerText": "Conservative Thai party looks to woo voters with promise to legalize sex toys"}, {"innerText": "Inside the Italian village being repopulated by Americans"}, {"innerText": "Strikes, soaring airfares and yo-yoing hotel fees: A traveler\\u2019s guide to the coronation"}, {"innerText": "A year in Azerbaijan: From spring\\u2019s Grand Prix to winter ski adventures"}, {"innerText": "The bicycle mayor peddling a two-wheeled revolution in Cape Town"}, {"innerText": "Tokyo ramen shop bans customers from using their phones while eating"}, {"innerText": "South African opera star will perform at coronation of King Charles III"}, {"innerText": "Luxury loot under the hammer: France auctions goods seized from drug dealers"}, {"innerText": "Judy Blume\\u2019s books were formative for generations of readers. Here\\u2019s why they endure"}, {"innerText": "Craft, salvage and sustainability take center stage at Milan Design Week"}, {"innerText": "Life-sized chocolate King Charles III sculpture unveiled to celebrate coronation"}, {"innerText": "Severe storms to strike the South again as millions in Texas could see damaging winds and hail"}, {"innerText": "The South is in the crosshairs of severe weather again, as the multi-day threat of large hail and tornadoes continues"}, {"innerText": "Spring snowmelt has cities along the Mississippi bracing for flooding in homes and businesses"}, {"innerText": "Know the difference between a tornado watch, a tornado warning and a tornado emergency"}, {"innerText": "Reporter spotted familiar face covering Sudan evacuation. See what happened next"}, {"innerText": "This country will soon become the world\\u2019s most populated"}, {"innerText": "April 27, 2023 - Russia-Ukraine news"}, {"innerText": "\\u2018Often they shoot at each other\\u2019: Ukrainian drone operator details chaos in Russian ranks"}, {"innerText": "Hear from family members of Americans stuck in Sudan frustrated with US response"}, {"innerText": "U.S. talk show host Jerry Springer dies at 79"}, {"innerText": "Bureaucracy stalling at least one family\\u2019s evacuation from Sudan"}, {"innerText": "Girl to get life-saving treatment for rare immune disease"}, {"innerText": "Haiti\\u2019s crime rate more than doubles in a year"}, {"innerText": "Ocean census aims to discover 100,000 previously unknown marine species"}, {"innerText": "Wall Street Journal editor discusses reporter\\u2019s arrest in Moscow"}, {"innerText": "Can Tunisia\\u2019s democracy be saved?"}, {"innerText": "Yasmeen Lari, \\u2018starchitect\\u2019 turned social engineer, wins one of architecture\\u2019s most coveted prizes"}, {"innerText": "A massive, newly restored Frank Lloyd Wright mansion is up for sale"}, {"innerText": "Are these the most sustainable architectural projects in the world?"}, {"innerText": "Step inside a $72 million London townhouse in a converted army barracks"}, {"innerText": "A 3D-printing company is preparing to build on the lunar surface. But first, a moonshot at home"}, {"innerText": "Simona Halep says \\u2018the stress is huge\\u2019 as she battles to return to tennis following positive drug test"}, {"innerText": "Barcelona reaches third straight Women\\u2019s Champions League final with draw against Chelsea"}, {"innerText": "Wrexham: An intoxicating tale of Hollywood glamor and sporting romance"}, {"innerText": "Shohei Ohtani comes within inches of making yet more MLB history in Angels win"}, {"innerText": "This CNN Hero is recruiting recreational divers to help rebuild reefs in Florida one coral at a time"}, {"innerText": "This CNN Hero offers judgment-free veterinary care for the pets of those experiencing homelessness"}, {"innerText": "Don\\u2019t give up on milestones: A CNN Hero\\u2019s message for Autism Awareness Month"}, {"innerText": "CNN Hero of the Year Nelly Cheboi returned to Kenya with plans to lift more students out of poverty"}]'

```










```
# If the agent wants to remember the current webpage, it can use the `current_webpage` tool
await tools_by_name['current_webpage'].arun({})

```








```
'https://web.archive.org/web/20230428133211/https://cnn.com/world'

```







