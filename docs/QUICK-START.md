# Quick Start Guide: GEO for Developers

> Get started with Generative Engine Optimization in 15 minutes

## What You'll Do

1. Add Schema.org markup to your blog post
2. Optimize code examples for AI citation
3. Test your content with AI tools

**Time required:** 15 minutes  
**Skill level:** Intermediate

---

## Step 1: Choose Your Content Type (2 min)

Identify what you're creating:

| Content Type | Use This Schema | Example |
|--------------|-----------------|---------|
| Technical article/blog post | TechArticle | Tutorial, guide, explanation |
| Step-by-step tutorial | HowTo | Deployment guide, setup tutorial |
| Q&A section | FAQPage | Common questions answered |
| API documentation | No schema needed | REST API docs |

---

## Step 2: Add Schema Markup (5 min)

### For a Technical Article

Add this in your `<head>` tag or before `</body>`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "TechArticle",
  "headline": "Your Article Title",
  "description": "Brief description of your article",
  "author": {
    "@type": "Person",
    "name": "Your Name",
    "jobTitle": "Your Title"
  },
  "datePublished": "2025-10-31",
  "dateModified": "2025-10-31"
}
</script>
```

**Customize:**
- Replace "Your Article Title" with your actual title
- Update the date to today's date (YYYY-MM-DD format)
- Add your name and job title

### For a Tutorial

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Deploy a Node.js App",
  "description": "Step-by-step deployment guide",
  "totalTime": "PT30M",
  "step": [
    {
      "@type": "HowToStep",
      "position": 1,
      "name": "Launch EC2 Instance",
      "text": "Go to AWS Console and launch an EC2 instance..."
    },
    {
      "@type": "HowToStep",
      "position": 2,
      "name": "Install Node.js",
      "text": "SSH into your instance and install Node.js..."
    }
  ]
}
</script>
```

---

## Step 3: Optimize Your Code Examples (5 min)

### ❌ Before (Not GEO-Optimized)

```javascript
const getData = async () => {
  const res = await fetch(url);
  return res.json();
};
```

### ✅ After (GEO-Optimized)

```javascript
/**
 * Fetches user data from API
 * @param {string} userId - The user's unique identifier
 * @returns {Promise<Object>} User object with id, name, and email
 * 
 * @example
 * const user = await fetchUserData('123');
 * console.log(user.name); // "John Doe"
 */
async function fetchUserData(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const userData = await response.json();
    return userData;
  } catch (error) {
    console.error('Error fetching user:', error);
    throw error;
  }
}
```

**Key improvements:**
- ✅ Descriptive function name
- ✅ JSDoc comments
- ✅ Parameter descriptions
- ✅ Return value documented
- ✅ Example usage
- ✅ Error handling

---

## Step 4: Test with AI Tools (3 min)

### Test Your Content

1. **Ask ChatGPT:** "How do I fetch user data with error handling in JavaScript?"

2. **Ask Claude:** "Show me an example of proper JSDoc documentation"

3. **Ask Perplexity:** "What's the best way to handle API errors in JavaScript?"

### What to Look For

✅ **Good signs:**
- Your site appears in citations
- Your code examples are referenced
- Your explanations are quoted (paraphrased)

❌ **Red flags:**
- Your content isn't mentioned
- Competitors are cited instead
- Your examples aren't used

---

## Step 5: Validate Your Markup (2 min)

### Check Schema

1. Go to [Schema Validator](https://validator.schema.org/)
2. Paste your JSON-LD
3. Fix any errors

### Check Rich Results

1. Go to [Rich Results Test](https://search.google.com/test/rich-results)
2. Enter your URL
3. Verify markup is detected

---

## Common Mistakes to Avoid

### 1. Missing Required Fields

```json
// ❌ BAD - Missing datePublished
{
  "@type": "TechArticle",
  "headline": "My Article"
}

// ✅ GOOD - All required fields
{
  "@type": "TechArticle",
  "headline": "My Article",
  "author": {"@type": "Person", "name": "John"},
  "datePublished": "2025-10-31"
}
```

### 2. Incomplete Code Examples

```javascript
// ❌ BAD - No context or explanation
const x = data.map(i => i * 2);

// ✅ GOOD - Clear and documented
// Double all values in the array
const doubledNumbers = numbers.map(num => num * 2);
```

### 3. No Author Information

```html
<!-- ❌ BAD - Anonymous article -->
<article>
  <h1>My Article</h1>
</article>

<!-- ✅ GOOD - Author specified -->
<article>
  <h1>My Article</h1>
  <address>
    By <a rel="author" href="/about">John Doe</a>
  </address>
</article>
```

---

## Next Steps

Now that you've optimized one piece of content:

1. **Apply to all content:** Update your existing posts
2. **Monitor results:** Check if AI tools start citing you
3. **Iterate:** See what works and refine
4. **Create templates:** Save schemas for reuse

---

## Cheat Sheet

### Schema.org Quick Reference

```javascript
// Blog post
{ "@type": "TechArticle" }

// Tutorial
{ "@type": "HowTo" }

// FAQ
{ "@type": "FAQPage" }

// API docs
{ "@type": "TechArticle", "articleSection": "API Documentation" }

// Benchmark/Research
{ "@type": "ScholarlyArticle" }
```

### Essential Properties

Every schema needs:
- `@context: "https://schema.org"`
- `@type: "..."`
- `headline` or `name`
- `author`
- `datePublished`

---

## Resources

- **Full Examples:** See `/examples` directory
- **Schema Docs:** https://schema.org/TechArticle
- **Validator:** https://validator.schema.org/
- **Checklist:** See `docs/GEO-CHECKLIST.md`

---

## Get Help

- **Questions?** Open an issue
- **Need examples?** Check the `/examples` directory
- **Want to contribute?** See `CONTRIBUTING.md`

---

**Congratulations!** You've taken your first steps into GEO optimization. Your content is now more discoverable by AI search engines.

**What's next?** Apply these principles to all your technical content and watch your citations grow! 📈