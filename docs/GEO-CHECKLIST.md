# GEO Optimization Checklist

Use this checklist to ensure your technical content is optimized for AI search engines and citations.

## 📝 Content Structure

### Semantic HTML
- [ ] Use `<article>` for main content
- [ ] Use `<section>` for logical content divisions
- [ ] Use `<header>` and `<footer>` appropriately
- [ ] Implement proper heading hierarchy (H1 → H2 → H3)
- [ ] Use `<code>` and `<pre>` for code examples
- [ ] Include `<time>` tags with datetime attributes
- [ ] Use `<address>` for author information

### Content Organization
- [ ] Clear, descriptive title (H1)
- [ ] Publication date visible and machine-readable
- [ ] Author name and credentials displayed
- [ ] Table of contents for long articles (>1000 words)
- [ ] Scannable with headers, lists, and short paragraphs
- [ ] Code examples are complete and runnable
- [ ] Include TL;DR or executive summary

## 🏷️ Schema Markup

### General Requirements
- [ ] Schema.org markup implemented (JSON-LD preferred)
- [ ] Validated with [Schema Validator](https://validator.schema.org/)
- [ ] Validated with [Rich Results Test](https://search.google.com/test/rich-results)

### TechArticle Schema
- [ ] `@type: TechArticle` specified
- [ ] `headline` included
- [ ] `description` included
- [ ] `author` with Person schema
- [ ] `datePublished` in ISO 8601 format
- [ ] `dateModified` when updated
- [ ] `publisher` information
- [ ] `keywords` array
- [ ] `citation` for sources (if applicable)
- [ ] `proficiencyLevel` specified

### HowTo Schema
- [ ] `@type: HowTo` specified
- [ ] `name` of the tutorial
- [ ] `description` of what will be accomplished
- [ ] `totalTime` estimated (ISO 8601 duration)
- [ ] `step` array with all steps
- [ ] Each step has `name` and `text`
- [ ] `supply` and `tool` lists (if applicable)

### FAQPage Schema
- [ ] `@type: FAQPage` specified
- [ ] Each Q&A wrapped in Question/Answer schema
- [ ] Questions have clear `name` property
- [ ] Answers have detailed `text` property

## 💻 Code Quality

### Documentation
- [ ] Every function has JSDoc or similar comments
- [ ] Parameters and return values documented
- [ ] Usage examples provided
- [ ] Edge cases and errors documented
- [ ] Dependencies clearly listed

### Code Examples
- [ ] Complete, not just fragments
- [ ] Include necessary imports
- [ ] Show error handling
- [ ] Include comments explaining key parts
- [ ] Provide both "bad" and "good" examples
- [ ] Use realistic variable names
- [ ] Include console.log or output examples

### Language-Specific
- [ ] JavaScript: Use modern ES6+ syntax
- [ ] Python: Follow PEP 8 style guide
- [ ] Include type hints where applicable
- [ ] Show async/await for asynchronous code

## 📚 API Documentation

### Endpoint Documentation
- [ ] Clear endpoint URL with base URL
- [ ] HTTP method specified (GET, POST, etc.)
- [ ] Authentication requirements stated
- [ ] Request headers documented
- [ ] Request body parameters in table format
- [ ] Required vs optional fields marked
- [ ] Data types specified
- [ ] Example requests in multiple formats (cURL, JavaScript, Python)
- [ ] Success response examples
- [ ] Error response examples with codes
- [ ] Rate limiting information

### Code Samples
- [ ] Examples in at least 2 languages
- [ ] Complete, copy-pasteable code
- [ ] Include error handling
- [ ] Show common use cases
- [ ] Demonstrate authentication

## 🎯 GEO-Specific Optimization

### Conversational Language
- [ ] Write in natural, conversational tone
- [ ] Answer "who, what, when, where, why, how"
- [ ] Use complete sentences
- [ ] Avoid excessive jargon
- [ ] Define technical terms on first use

### Credibility Signals
- [ ] Author credentials visible
- [ ] Publication date clearly displayed
- [ ] Last updated date shown
- [ ] Sources cited for claims
- [ ] Link to authoritative references
- [ ] Include methodology for benchmarks

### Originality
- [ ] Original research or analysis
- [ ] Unique code examples
- [ ] Personal insights or experiences
- [ ] Novel comparisons or benchmarks
- [ ] Custom diagrams or visualizations

### Scannability
- [ ] Headers every 200-300 words
- [ ] Bullet points for lists
- [ ] Tables for structured data
- [ ] Bold key terms and phrases
- [ ] Short paragraphs (3-5 sentences)
- [ ] Visual hierarchy clear

## 🔍 Technical SEO (Still Relevant)

### Meta Tags
- [ ] Title tag (50-60 characters)
- [ ] Meta description (150-160 characters)
- [ ] Canonical URL specified
- [ ] Open Graph tags for social sharing
- [ ] Twitter Card tags

### Performance
- [ ] Images optimized and compressed
- [ ] Lazy loading for images
- [ ] Code syntax highlighting lightweight
- [ ] Minimize external dependencies
- [ ] Fast page load time (<3 seconds)

### Accessibility
- [ ] Alt text for images
- [ ] ARIA labels where needed
- [ ] Sufficient color contrast
- [ ] Keyboard navigation works
- [ ] Screen reader friendly

## 📊 Original Research & Benchmarks

### Methodology
- [ ] Test environment documented
- [ ] Hardware/software specifications listed
- [ ] Sample size stated
- [ ] Test procedure explained
- [ ] Variables controlled
- [ ] Reproducible steps provided

### Results
- [ ] Data presented in tables
- [ ] Visual representations (charts/graphs)
- [ ] Statistical significance noted
- [ ] Confidence intervals included
- [ ] Limitations acknowledged
- [ ] Raw data available

### Reproducibility
- [ ] Code repository linked
- [ ] Test scripts available
- [ ] Setup instructions provided
- [ ] Dependencies documented

## 🔄 Maintenance

### Updates
- [ ] Review content quarterly
- [ ] Update outdated information
- [ ] Fix broken links
- [ ] Update version numbers
- [ ] Revise deprecated code
- [ ] Add note about changes

### Monitoring
- [ ] Track which AI tools cite your content
- [ ] Monitor referral traffic
- [ ] Check for mentions
- [ ] Gather user feedback
- [ ] Analyze which sections are cited most

## 📱 Multi-Platform Optimization

### Mobile
- [ ] Responsive design
- [ ] Touch-friendly buttons
- [ ] Readable font sizes
- [ ] No horizontal scrolling

### Different AI Tools
- [ ] Test in ChatGPT
- [ ] Test in Claude
- [ ] Test in Perplexity
- [ ] Test in Google AI Overviews
- [ ] Check which content gets cited

## 🎓 Educational Value

### Learning Path
- [ ] Prerequisites stated upfront
- [ ] Progressive difficulty
- [ ] Concepts explained before implementation
- [ ] Links to prerequisite knowledge

### Completeness
- [ ] All necessary steps included
- [ ] No assumed knowledge (or stated)
- [ ] Common errors addressed
- [ ] Troubleshooting section
- [ ] Next steps suggested

## ✅ Final Pre-Publish Checks

### Quality Assurance
- [ ] Proofread for typos and grammar
- [ ] All links work
- [ ] All code examples tested
- [ ] Images load correctly
- [ ] Schema markup validated
- [ ] HTML validated
- [ ] Accessibility tested

### GEO Verification
- [ ] Asked AI tools about your topic
- [ ] Checked if similar content gets cited
- [ ] Confirmed your unique angle
- [ ] Verified all sources are authoritative

---

## Scoring Guide

Count your checkmarks:

- **90-100%** - Excellent! Your content is highly optimized for GEO
- **75-89%** - Good. A few improvements will boost GEO performance
- **60-74%** - Decent. Focus on schema markup and code documentation
- **Below 60%** - Needs work. Start with content structure and schema

---

**Remember:** Not every item applies to every piece of content. Use this as a guide, not a rigid requirement.

**Pro Tip:** Focus first on items that provide the most GEO value:
1. Schema markup (TechArticle, HowTo, FAQPage)
2. Complete, documented code examples
3. Original research or insights
4. Clear, conversational explanations

---

*Save this checklist and use it for every technical article or documentation you create!*