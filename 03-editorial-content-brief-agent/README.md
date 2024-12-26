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

This LangFlow flow consists of the following components:

1.  **Chat Input:** Takes user input, which is the topic for which you want to create a content plan.
2.  **Prompt (Get Top Editorial SERP):** Formats a prompt to instruct the agent to use Tavily Search to investigate the top 3 results for the query and analyze the findings.
3.  **Tavily AI Search:** Uses the Tavily API to search for the given topic and retrieve the top 3 results.
4.  **Agent (Get Top Editorial SERP Agent):** Analyzes the search results from Tavily, ensuring the pages are informational intent (not product pages, commercial category pages, or website homepages).
5.  **Prompt (Competitive Page Content Analysis):** Formats a prompt to instruct the agent to scrape, organize, and summarize content from URLs, including headers, key points, subheadings, and links.
6.  **Firecrawl Scrape API Tool:** Uses the Firecrawl API to scrape the content from the URLs identified by the agent.
7.  **Agent (Competitive Page Content Analysis Agent):** Uses an LLM to scrape, organize, and summarize content from the URLs, including headers, key points, subheadings, and links.
8.  **Custom Component (PageKeywordsExtractorTool):** Uses the DataForSEO API to extract keywords for each URL.
9.  **Agent (Competitive Keyword Extraction Agent):** Uses an LLM to extract and organize keywords from URLs into a markdown table.
10. **Prompt (Links Prompt):** Formats a prompt to consolidate the links from across the outputs.
11. **Prompt (Recurring Topics Prompt):** Formats a prompt to analyze the content outlines to identify recurring topics.
12. **Combine Text:** Combines the outputs from the agents and prompts into a single text chunk.
13. **OpenAI Model (for Content Outline):** Uses an LLM to generate a detailed content outline based on the combined text.
14. **Prompt (Primary Header Creation Prompt):** Formats a prompt to create an SEO-optimized and compelling H1 heading for the article.
15. **OpenAI Model (for H1 Creation):** Uses an LLM to generate an SEO-optimized and compelling H1 heading for the article.
16. **Custom Component (CleanMarkdown):** Uses the textwrap python module to clean the final markdown output.
17. **Chat Output:** Displays the final content outline and H1 heading in a user-friendly format.

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

- A detailed content outline, including main section headers (H2s), sub-section headers (H3s), content outlines for each section, and media ideas.
- A consolidated list of external links found on competitive pages.
- A list of recurring topics identified across competitive pages, including the pages where they appear and a summary of their importance.
- A list of key takeaways for optimizing the content.
- A markdown table listing URLs and their associated target keywords with search volume.
- A single H1 heading for the article.

**Example Output:**

```
## Content Outline:

**H2: Packing Essentials for Iceland in Winter**
    -   **H3: Clothing Layers**
        -   **Content Outline**: Discuss the importance of layering, including base layers, mid-layers, and outer layers.
    -   **H3: Footwear**
        -   **Content Outline:** Recommend waterproof and insulated boots with good traction.
    -   **Media Ideas:** Infographic showing the different layers of clothing.

**H2: Planning Your Itinerary**
    -   **H3: Popular Destinations**
        -   **Content Outline:** Highlight popular destinations like the Blue Lagoon, Golden Circle, and Northern Lights viewing spots.
    -   **H3: Transportation Options**
        -   **Content Outline:** Discuss renting a car, joining a tour, or using public transportation.
    -   **Media Ideas:** Map of Iceland with popular destinations marked.

## Consolidated Links:

| Link                                                                                     | Found on                                                                           |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| [https://www.example1.com/packing-list](https://www.example1.com/packing-list) | [https://www.example1.com/winter-travel](https://www.example1.com/winter-travel) |
| [https://www.example2.com/destinations](https://www.example2.com/destinations) | [https://www.example2.com/planning-tips](https://www.example2.com/planning-tips) |

## Recurring Topics:

**Winter Clothing**

   - Pages Mentioning Topic 1: [https://www.example1.com/winter-travel](https://www.example1.com/winter-travel), [https://www.example2.com/packing-list](https://www.example2.com/packing-list)
   - Reason or Summary: The importance of layering and waterproof gear is consistently emphasized.

**Transportation**

   - Pages Mentioning Topic 2: [https://www.example1.com/winter-travel](https://www.example1.com/winter-travel), [https://www.example2.com/planning-tips](https://www.example2.com/planning-tips)
   - Reason or Summary: The need for reliable transportation is a key concern for winter travel in Iceland.

## Insights for SEO Optimization:

- **Keyword Focus**: Prioritize keywords related to "winter travel in Iceland," "packing for Iceland," and "Iceland winter destinations."
- **Content Depth**: Provide detailed information on each topic, including specific recommendations and tips.

## Competitive Keyword Targets:

| URL                     | Keywords                                                     |
|-------------------------|--------------------------------------------------------------|
| https://www.example1.com     | iceland winter travel (1200 searches/mo), packing for iceland (950 searches/mo), ...      |
| https://www.example2.com | iceland destinations (500 searches/mo), winter activities (300 searches/mo), ...  |

# How to Prepare for a Trip to Iceland During the Winter?
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
