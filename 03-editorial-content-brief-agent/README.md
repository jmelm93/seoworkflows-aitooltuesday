# Week 3 - Editorial Content Briefing Agent

## Overview

This tool is designed to help with content mapping and planning for SEO. It leverages AI to perform competitive analysis, aggregate keyword targets, and provide an optimized content outline based on a given topic.

**Key Features:**

- Uses Tavily AI Search to identify top-ranking pages for a given topic.
- Scrapes and analyzes content from multiple URLs using Firecrawl API.
- Extracts and organizes keywords using DataForSEO API.
- Identifies recurring topics and provides actionable insights for SEO optimization.
- Generates a detailed, SEO-optimized content outline, including headers, subheadings, and media ideas.

## How It Works

This LangFlow flow is structured with an initial split into two parallel flows that converge for final processing:

```mermaid
graph LR
    A[User Input] --> B(Initial SERP Analysis);
    B --> C(Content Scraping and Summarization);
    B --> D(Keyword Extraction);
    C --> E(Data Consolidation and Preparation);
    D --> F(Content Outline Generation);
    E --> F;
    F --> G(H1 Heading Creation);
    G --> H(Output Formatting);
    H --> I[Chat Output];

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style I fill:#ccf,stroke:#333,stroke-width:2px
```

1.  **User Input:** The process begins with a user providing a topic for which they want to create a content plan via the **Chat Input** node.

2.  **Initial SERP Analysis:** This step focuses on identifying relevant content. It includes:

    - **Prompt (Get Top Editorial SERP):** Formats a prompt to guide the agent.
    - **Tavily AI Search:** Uses the Tavily API to search for the given topic and retrieve the top 3 results.
    - **Agent (Get Top Editorial SERP Agent):** Analyzes the search results, ensuring the pages are informational and not commercial.

3.  **Flow 1: Content Scraping and Summarization:** This flow focuses on extracting and summarizing content from the identified URLs. It includes:

    - **Prompt (Competitive Page Content Analysis):** Formats a prompt to guide the agent.
    - **Firecrawl Scrape API Tool:** Uses the Firecrawl API to scrape the content from the URLs.
    - **Agent (Competitive Page Content Analysis Agent):** Uses an LLM to scrape, organize, and summarize content from the URLs.

4.  **Flow 2: Keyword Extraction:** This flow focuses on identifying relevant keywords for the target page. It includes:

    - **Custom Component (PageKeywordsExtractorTool):** Uses the DataForSEO API to extract keywords for each URL.
    - **Agent (Competitive Keyword Extraction Agent):** Uses an LLM to extract and organize keywords from URLs into a markdown table.

5.  **Data Consolidation and Preparation:** This step prepares the data for the content outline generation. It includes:

    - **Prompt (Links Prompt):** Formats a prompt to consolidate the links from across the outputs.
    - **Prompt (Recurring Topics Prompt):** Formats a prompt to analyze the content outlines to identify recurring topics.
    - **Combine Text:** Combines the outputs from the agents and prompts into a single text chunk.

6.  **Content Outline Generation:** This step uses an LLM to generate a detailed content outline, incorporating the keyword data. It includes:

    - **OpenAI Model (for Content Outline):** Uses an LLM to generate a detailed content outline based on the combined text.

7.  **H1 Heading Creation:** This step focuses on creating an SEO-optimized H1 heading. It includes:

    - **Prompt (Primary Header Creation Prompt):** Formats a prompt to create an SEO-optimized and compelling H1 heading for the article.
    - **OpenAI Model (for H1 Creation):** Uses an LLM to generate an SEO-optimized and compelling H1 heading for the article.

8.  **Output Formatting:** This step cleans up the final output. It includes:
    - **Custom Component (CleanMarkdown):** Uses the textwrap python module to clean the final markdown output.
    - **Chat Output:** Displays the final content outline and H1 heading in a user-friendly format.

## Environment Variables

To use this tool, you need to set the following environment variables in your LangFlow instance:

- **`TAVILY_API_KEY`**: Your Tavily API key. You can obtain one from the Tavily website: [https://tavily.com/](https://tavily.com/)
- **`FIRECRAWL_API_KEY`**: Your Firecrawl API key. You can obtain one from the Firecrawl website: [https://firecrawl.dev/](https://firecrawl.dev/)
- **`OPENAI_API_KEY`**: Your OpenAI API key. You can obtain one from the OpenAI website: [https://platform.openai.com/](https://platform.openai.com/)
- **`DATAFORSEO_USERNAME`**: Your DataForSEO username. You can obtain one from the DataForSEO website: [https://dataforseo.com/](https://dataforseo.com/)
- **`DATAFORSEO_PASSWORD`**: Your DataForSEO password. You can obtain one from the DataForSEO website: [https://dataforseo.com/](https://dataforseo.com/)

**How to Set Environment Variables:**

1.  After uploading the flow to LangFlow, click on the "Settings" icon (usually a gear icon) in the top right corner.
2.  Navigate to the "Environment Variables" section.
3.  Add the required variables with their corresponding values.

## Input Format

The tool expects a single input: a topic for which you want to create a content plan.

**Example Input:**

```
how to prepare for a trip to iceland during the winter?
```

## Output Format

The tool will output a markdown formatted response that includes:

- A single H1 heading for the article.
- A markdown table listing competitive URLs and their associated target keywords with search volume.
- A list of recurring topics identified across competitive pages, including the pages where they appear and a summary of their importance.
- A consolidated list of links found on competitive pages.
- A list of key takeaways for optimizing the content.
- The competitive page review, including the top 3 competitors and the outlines from their pages.
- A detailed content outline, including main section headers (H2s), sub-section headers (H3s), content outlines for each section, and media ideas.

**Example Output:**

```
# Iceland in Winter: Essential Packing List and Travel Tips

## Competitive Keyword Targets:

| URL                                                         | Keywords                                                                                           |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| https://adventuresbylana.com/iceland-winter-packing-list/   | packing list for iceland winter (320 searches/mo), iceland packing list (1300 searches/mo), iceland pack list (1300 searches/mo), what to pack for iceland in january (40 searches/mo), what to wear in iceland december (170 searches/mo) |
| https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/ | winter iceland packing list (320 searches/mo), winter packing list for iceland (320 searches/mo), iceland winter packing list (320 searches/mo), what to take to iceland in december (170 searches/mo), what to bring to iceland in january (140 searches/mo) |
| https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide | iceland in the winter (2900 searches/mo), winter iceland (2900 searches/mo), iceland at winter (2900 searches/mo), icelandic winter (2900 searches/mo), iceland during winter (2900 searches/mo) |

## Recurring Topics:

**Winter Weather in Iceland**

   - Pages Mentioning Topic: [Adventures by Lana](https://adventuresbylana.com/iceland-winter-packing-list/), [Iceland Travel Guide](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/), [Guide to Iceland](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide)
   - Reason or Summary: Understanding the winter weather in Iceland is crucial for travelers to pack appropriately and plan their activities. This topic is consistently covered to help readers prepare for the cold, precipitation, and limited daylight.

**Packing List for Icelandic Winters**

   - Pages Mentioning Topic: [Adventures by Lana](https://adventuresbylana.com/iceland-winter-packing-list/), [Iceland Travel Guide](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/)
   - Reason or Summary: A detailed packing list is essential for travelers to ensure they have the necessary clothing and gear to stay warm and dry in Iceland's harsh winter conditions. This topic is a focal point for providing practical advice to travelers.

**Layering and Clothing Recommendations**

   - Pages Mentioning Topic: [Adventures by Lana](https://adventuresbylana.com/iceland-winter-packing-list/), [Iceland Travel Guide](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/)
   - Reason or Summary: Layering is a key strategy for staying warm in cold climates, and both pages emphasize the importance of choosing the right base, mid, and outer layers. This advice is crucial for comfort and safety in winter travel.

**Winter Activities in Iceland**

   - Pages Mentioning Topic: [Guide to Iceland](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide)
   - Reason or Summary: Highlighting winter activities such as viewing the Northern Lights and glacier hiking attracts tourists interested in unique experiences. This topic is important for promoting Iceland as a winter travel destination.

## Insights for SEO Optimization:

- **Focus on Weather-Related Content**: Create detailed guides on Iceland's winter weather, including monthly breakdowns and safety tips, to attract search traffic from travelers planning their trips.
- **Develop Comprehensive Packing Lists**: Offer downloadable or interactive packing lists tailored to different types of travelers (e.g., adventure seekers, families) to increase engagement and shareability.
- **Emphasize Layering Techniques**: Provide in-depth articles or videos on effective layering strategies, including product recommendations, to position the site as an authority on winter travel preparation.
- **Promote Unique Winter Activities**: Highlight and optimize content around popular winter activities in Iceland, using keywords related to these experiences to capture interest from adventure tourists.

## Consolidated Links:

| Link                                                                                     | Found on                                                                           |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| [https://adventuresbylana.com](https://adventuresbylana.com)                             | [https://adventuresbylana.com/iceland-winter-packing-list/](https://adventuresbylana.com/iceland-winter-packing-list/) |
| [https://tripadvisor.com](https://tripadvisor.com)                                       | [https://adventuresbylana.com/iceland-winter-packing-list/](https://adventuresbylana.com/iceland-winter-packing-list/) |
| [https://amazon.com](https://amazon.com)                                                 | [https://adventuresbylana.com/iceland-winter-packing-list/](https://adventuresbylana.com/iceland-winter-packing-list/) |
| [https://icelandtravelguide.is](https://icelandtravelguide.is)                           | [https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/) |
| [https://facebook.com](https://facebook.com)                                             | [https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/) |
| [https://wikimedia.org](https://wikimedia.org)                                           | [https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/) |
| [https://guidetoiceland.is](https://guidetoiceland.is)                                   | [https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide) |
| [https://youtube.com](https://youtube.com)                                               | [https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide) |
| [https://vedur.is](https://vedur.is)                                                     | [https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide) |

## Competitive Page Review:

### URL: [https://adventuresbylana.com/iceland-winter-packing-list/](https://adventuresbylana.com/iceland-winter-packing-list/)

**Section: The Ultimate Iceland Winter Packing List**

- **Key Points:**
  - Comprehensive packing list for a winter trip to Iceland.
  - Emphasis on layering clothing to stay warm.
  - Recommendations for specific clothing items and accessories.
- **Section Subheadings:**
  - **Quick Tips for Staying Warm in Iceland**: Tips on using hand warmers, layering, and staying dry.
  - **Winter Clothing You’ll Need for Iceland**: Detailed breakdown of base layers, mid-layers, and outer layers.
  - **Accessories for Winter in Iceland**: Importance of hats, gloves, scarves, and socks.
  - **Winter Weather in Iceland: Breakdown by Month**: Overview of weather conditions from November to February.

---

### URL: [https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/](https://icelandtravelguide.is/blog-posts/iceland-winter-packing-list/)

**Section: Know the Winter Weather in Iceland for Packing Suitably**

- **Key Points:**
  - Importance of understanding Iceland's winter weather for packing.
  - Average temperatures and precipitation details.
  - Daylight hours and their impact on travel plans.
- **Section Subheadings:**
  - **Temperature**: Average winter temperatures and their implications.
  - **Precipitation**: Expected rainfall and snowfall during winter.
  - **Daylight Hours**: Short daylight hours and their effect on travel.

**Section: Packing List Items for Icelandic Winters**

- **Key Points:**
  - Detailed clothing and accessory recommendations.
  - Emphasis on layering and waterproof clothing.
  - Suggestions for footwear and tech gear.
- **Section Subheadings:**
  - **Clothes**: Breakdown of essential clothing items.
  - **Shoes**: Recommendations for waterproof boots and crampons.
  - **Other Clothing Accessories**: Importance of socks, hats, gloves, and scarves.

---

### URL: [https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide](https://guidetoiceland.is/best-of-iceland/iceland-in-winter-the-ultimate-travel-guide)

**Section: What to Do in Winter in Iceland**

- **Key Points:**
  - Overview of winter activities in Iceland.
  - Recommendations for seeing the Northern Lights and glacier hiking.
  - Importance of flexible travel plans due to weather.
- **Section Subheadings:**
  - **See the Northern Lights**: Best practices for viewing the aurora.
  - **Go Glacier Hiking or Ice Caving**: Safety tips and tour recommendations.
  - **Visit the Golden Circle**: Highlights of this popular tourist route.

**Section: Winter Weather in Iceland**

- **Key Points:**
  - Description of Iceland's winter climate and its challenges.
  - Average temperatures and weather conditions by month.
  - Safety tips for dealing with winter weather.
- **Section Subheadings:**
  - **Average Temperature in November**: Temperature ranges and conditions.
  - **Average Temperature in December**: Details on December's climate.
  - **Safety in Winter**: Tips for staying safe during winter travel.

## Content Outline:

**H2: Understanding Iceland's Winter Weather**
   - **H3: Monthly Weather Breakdown**
       - **Content Outline**: Provide detailed information on average temperatures, precipitation, and daylight hours for each winter month (November to February). Include safety tips for travelers.
   - **H3: Weather Challenges and Safety Tips**
       - **Content Outline**: Discuss the challenges posed by Iceland's winter weather, such as icy roads and limited daylight. Offer practical safety tips for travelers.
   - **Media Ideas:** Infographic showing average temperatures and daylight hours by month; video on winter safety tips.

**H2: Essential Packing List for Icelandic Winters**
   - **H3: Clothing Essentials**
       - **Content Outline**: Detail the necessary clothing items, focusing on base layers, mid-layers, and outer layers. Emphasize the importance of layering for warmth.
   - **H3: Footwear and Accessories**
       - **Content Outline**: Recommend waterproof boots, crampons, and essential accessories like hats, gloves, and scarves. Highlight the importance of staying dry and warm.
   - **H3: Tech Gear and Miscellaneous Items**
       - **Content Outline**: Suggest tech gear such as cameras and power banks, and other useful items like hand warmers and travel guides.
   - **Media Ideas:** Interactive packing checklist; images of recommended clothing and gear.

**H2: Layering Techniques for Winter Travel**
   - **H3: Understanding Layering**
       - **Content Outline**: Explain the concept of layering and its benefits for winter travel. Discuss the role of each layer in maintaining warmth and comfort.
   - **H3: Product Recommendations**
       - **Content Outline**: Provide specific product recommendations for each layer, including materials and brands known for quality winter gear.
   - **Media Ideas:** Video tutorial on effective layering techniques; comparison chart of different layering materials.

**H2: Winter Activities in Iceland**
   - **H3: Northern Lights Viewing**
       - **Content Outline**: Offer tips for the best times and locations to view the Northern Lights. Include safety considerations and photography tips.
   - **H3: Glacier Hiking and Ice Caving**
       - **Content Outline**: Describe the experience of glacier hiking and ice caving, including necessary gear and safety precautions. Recommend reputable tour operators.
   - **H3: Exploring the Golden Circle**
       - **Content Outline**: Highlight the key attractions of the Golden Circle and provide tips for visiting during winter. Discuss the impact of weather on travel plans.
   - **Media Ideas:** Photo gallery of winter activities; video guide to the Northern Lights.

**H2: Planning Your Winter Trip to Iceland**
   - **H3: Flexible Itinerary Tips**
       - **Content Outline**: Advise on creating a flexible itinerary to accommodate weather changes. Include tips for booking accommodations and tours.
   - **H3: Budgeting for Winter Travel**
       - **Content Outline**: Discuss the costs associated with winter travel in Iceland, including flights, accommodations, and activities. Offer budgeting tips.
   - **Media Ideas:** Sample winter itinerary; budgeting calculator for trip planning.
```

## Usage Tips

- Ensure that your API keys are valid and have sufficient usage credits.
- The tool will take a few minutes to run, depending on the complexity of the topic and the number of URLs it needs to analyze.
- Review the output carefully and make any necessary adjustments to the suggested content outline and H1 heading.
- The output is designed to be a starting point for your content creation process.

## Stay Updated

Follow us to stay updated on the latest AI SEO tools released every Tuesday!

- **Blog:** [https://www.seoworkflows.com/blog](https://www.seoworkflows.com/blog)
- **YouTube:** [https://www.youtube.com/@seoworkflows](https://www.youtube.com/@seoworkflows)
- **LinkedIn:** [https://www.linkedin.com/company/seo-workflows/](https://www.linkedin.com/company/seo-workflows/)

Happy testing! 🚀
