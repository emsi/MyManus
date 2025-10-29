# Example 3: Web Automation & Data Extraction

## Task Description

**User Request:**
```
Go to Hacker News (news.ycombinator.com), find the top 10 stories,
and for each story, extract the title, score, number of comments,
and the article URL. Compile the data into a structured CSV file.
```

## Expected Behavior with MyManus Plugin

This example demonstrates the power of Playwright MCP integration for complex web interactions.

### Phase 1: Planning (TodoWrite)

Claude creates an automated data extraction plan:

```
✓ Creating web scraping todo list
  - Use browser to navigate to Hacker News
  - Extract top 10 story data (title, score, comments, URL)
  - Handle any popups or banners
  - Verify data extraction completeness
  - Compile data into structured format
  - Save to CSV file
  - Validate CSV output
```

### Phase 2: Browser Automation

#### Step 1: Initial Navigation

Claude uses Playwright MCP to launch browser and navigate:

```
Opening browser...
Navigating to news.ycombinator.com...
Page loaded successfully
Waiting for content to render...
```

**Autonomous behavior:** Claude handles browser launch and navigation automatically.

#### Step 2: Intelligent Data Extraction

Claude interacts with the page programmatically:

```python
# Claude generates and executes browser automation code
stories = []

# Find story elements
story_elements = page.query_selector_all('.athing')

for i, story in enumerate(story_elements[:10]):
    story_id = story.get_attribute('id')

    # Extract title and URL
    title_link = story.query_selector('.titleline > a')
    title = title_link.inner_text()
    url = title_link.get_attribute('href')

    # Get corresponding score and comment count
    subtext = page.query_selector(f'#score_{story_id}')

    # Parse score (e.g., "123 points")
    score_text = subtext.query_selector('.score').inner_text()
    score = int(score_text.split()[0])

    # Parse comments (e.g., "45 comments")
    comments_link = subtext.query_selector_all('a')[-1]
    comments_text = comments_link.inner_text()

    if 'comment' in comments_text:
        comments = int(comments_text.split()[0])
    else:
        comments = 0

    stories.append({
        'rank': i + 1,
        'title': title,
        'url': url,
        'score': score,
        'comments': comments
    })

print(f"Extracted {len(stories)} stories")
```

**Autonomous behavior:**
- Claude writes the extraction code automatically
- Handles variations in page structure
- Deals with missing data gracefully

#### Step 3: Data Validation

Claude validates extracted data:

```
Validating extracted data...
✓ All 10 stories extracted
✓ All fields present for each story
✓ URLs are valid
✓ Scores are numeric
✓ Comment counts are numeric
```

**Autonomous behavior:** Claude verifies data quality before proceeding.

### Phase 3: Data Processing & Export

#### Step 1: Create Structured Data

Claude organizes the extracted data:

```python
import csv
from datetime import datetime

# Prepare data for CSV
csv_filename = f"hackernews_top10_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv"

headers = ['Rank', 'Title', 'URL', 'Score', 'Comments']

with open(csv_filename, 'w', newline='', encoding='utf-8') as csvfile:
    writer = csv.DictWriter(csvfile, fieldnames=headers)
    writer.writeheader()

    for story in stories:
        writer.writerow({
            'Rank': story['rank'],
            'Title': story['title'],
            'URL': story['url'],
            'Score': story['score'],
            'Comments': story['comments']
        })

print(f"Data saved to {csv_filename}")
```

#### Step 2: Create Summary Report

Claude automatically creates a summary:

```markdown
# Hacker News Top 10 Stories
*Extracted on: 2024-10-29 15:30:45*

## Summary Statistics

- Total stories analyzed: 10
- Average score: 287 points
- Average comments: 156 comments
- Most discussed: "Story Title Here" (543 comments)
- Highest score: "Another Story" (892 points)

## Top Stories

1. **Story Title 1** (892 points, 234 comments)
   https://example.com/article1

2. **Story Title 2** (678 points, 543 comments)
   https://example.com/article2

[... etc ...]

## Data Files

- Raw data: hackernews_top10_20241029_153045.csv
- Summary: hackernews_summary_20241029_153045.md
```

**Autonomous behavior:**
- Claude creates both raw data and human-readable summary
- Adds timestamps and metadata
- Calculates useful statistics

### Phase 4: Final Output

#### CSV File (hackernews_top10_20241029_153045.csv)

```csv
Rank,Title,URL,Score,Comments
1,Groundbreaking AI Research Paper,https://arxiv.org/paper,892,234
2,New Programming Language Released,https://newlang.dev,678,543
3,Startup Raises $50M Series A,https://techcrunch.com/article,564,87
4,Show HN: I Built a Cool Tool,https://github.com/user/tool,445,312
5,Security Vulnerability Discovered,https://security.blog/vuln,398,289
6,Ask HN: Best Practices for X,https://news.ycombinator.com/item?id=123,334,445
7,Interesting Technical Deep Dive,https://blog.com/deep-dive,298,156
8,Company Announces Major Feature,https://company.com/blog,276,98
9,Open Source Project Milestone,https://github.com/project,245,134
10,Industry Analysis and Trends,https://analyst.com/report,198,76
```

## Key Plugin Features Demonstrated

1. **Browser Automation**: Full Playwright integration for complex interactions
2. **Intelligent Extraction**: Claude figures out the extraction logic automatically
3. **Error Handling**: Gracefully handles missing data or page changes
4. **Data Validation**: Verifies extracted data before saving
5. **Multiple Output Formats**: Creates both CSV (machine) and Markdown (human)
6. **Automatic Analysis**: Calculates statistics without being asked
7. **Progress Tracking**: TodoWrite updates throughout the process

## Advanced Web Automation Examples

### Example 2: Multi-Page Navigation

**User Request:**
```
Go to GitHub, search for "machine learning" repositories,
sort by stars, and extract the top 20 repositories with their
star counts, descriptions, and primary languages.
```

**What Claude Does:**
1. Opens browser to github.com
2. Handles any cookie banners
3. Enters search term in search box
4. Clicks sort dropdown, selects "Most stars"
5. Iterates through results, handling pagination if needed
6. Extracts data for each repository
7. Compiles into structured format
8. Creates visualization of language distribution

**Autonomous behavior:** Claude handles complex multi-step interactions without step-by-step instructions.

### Example 3: Form Interaction

**User Request:**
```
Use the browser to go to the Python Package Index (pypi.org),
search for packages related to "data visualization", and compile
a comparison table of the top 5 results including download counts,
last update date, and Python version support.
```

**What Claude Does:**
1. Navigates to pypi.org
2. Enters search term in form
3. Submits search
4. For each of top 5 results:
   - Clicks through to package page
   - Extracts detailed information
   - Returns to search results
5. Compiles comparison table
6. Adds analysis and recommendations

### Example 4: Dynamic Content Handling

**User Request:**
```
Go to a JavaScript-heavy news site, scroll through the page to
load more articles (infinite scroll), and extract headlines and
publication dates for at least 50 articles.
```

**What Claude Does:**
1. Opens page and waits for initial load
2. Detects infinite scroll pattern
3. Scrolls incrementally, waiting for content to load
4. Extracts data as new content appears
5. Continues until 50 articles collected
6. Deduplicates if necessary
7. Saves to structured format with timestamps

**Autonomous behavior:** Claude figures out the page's lazy-loading behavior and adapts the extraction strategy accordingly.

## User Experience

### What You See

```
User: Go to Hacker News and extract the top 10 stories...

Claude: I'll use the browser to extract the top 10 stories from
Hacker News and compile them into a CSV file.

[TodoWrite creates task plan]

Opening browser and navigating to Hacker News...
✓ Page loaded

Extracting story data...
✓ Extracted 10 stories

Validating data...
✓ All data complete and valid

Creating CSV file...
✓ hackernews_top10_20241029_153045.csv created

Creating summary report...
✓ hackernews_summary_20241029_153045.md created

Claude: I've successfully extracted the top 10 stories from Hacker News.

Data files created:
- hackernews_top10_20241029_153045.csv (raw data)
- hackernews_summary_20241029_153045.md (human-readable summary)

Summary: Average score is 287 points with 156 average comments.
Most discussed story is "Story Title Here" with 543 comments.
```

### What You Don't Need To Do

- ❌ Tell Claude which browser automation tool to use
- ❌ Provide CSS selectors or XPath expressions
- ❌ Write extraction code
- ❌ Handle errors or missing data
- ❌ Format the output
- ❌ Create summary statistics

**Claude handles the entire web automation workflow autonomously.**

## Comparison: With vs Without Plugin

### Without MyManus Plugin + Playwright

**User:** Extract top 10 stories from Hacker News...

**Claude:** I can help you write a web scraping script using BeautifulSoup:

```python
import requests
from bs4 import BeautifulSoup

# [Basic code example]
# User must run this code themselves
```

**Limitations:**
- Claude provides code, doesn't execute
- No browser automation (only static HTML)
- User must debug and run
- No data validation
- Basic output only

### With MyManus Plugin + Playwright

**User:** Extract top 10 stories from Hacker News...

**Claude:** [Opens actual browser and executes]

**Advantages:**
- Real browser automation with JavaScript support
- Claude executes the extraction
- Handles dynamic content
- Validates data automatically
- Creates multiple output formats
- Provides analysis and insights
- Deals with errors autonomously

## WebFetch vs Playwright: When to Use Which

The MyManus plugin enables Claude to choose the right tool:

### Use WebFetch When:
- Simple page retrieval
- Static HTML content
- Text extraction and analysis
- Documentation reading
- Quick page checks

**Example:**
```
"What does the Python documentation say about asyncio?"
```
→ Claude uses WebFetch (faster, simpler)

### Use Playwright When:
- Complex interactions (clicking, typing, scrolling)
- JavaScript-heavy pages
- Multi-step navigation
- Form submissions
- Dynamic content loading
- Handling popups/banners

**Example:**
```
"Search GitHub for 'rust web frameworks' and extract the top results"
```
→ Claude uses Playwright (requires interaction)

**Autonomous behavior:** Claude automatically selects the appropriate tool based on task requirements.

## Best Practices for Web Automation

When using the MyManus plugin for web automation:

1. **Be Specific About Data Needs**: Clearly specify what data to extract
2. **Allow Adaptation**: Claude may need to adjust approach based on page structure
3. **Trust Error Handling**: Claude will handle common issues (popups, missing data)
4. **Request Multiple Formats**: Ask for both raw data and analysis
5. **Consider Rate Limiting**: For large extractions, Claude will pace requests appropriately

## Ethical Considerations

The plugin maintains ethical web scraping practices:

- ✅ Respects robots.txt
- ✅ Adds appropriate delays
- ✅ Uses respectful User-Agent strings
- ✅ Doesn't overwhelm servers
- ✅ Follows site terms of service

Claude will warn you if a requested action might violate these principles.

---

**Web automation with the MyManus plugin transforms Claude from a code generator into an active web agent.**
