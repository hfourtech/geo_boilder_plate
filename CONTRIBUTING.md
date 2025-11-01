# Contributing to GEO Optimization Guide

Thank you for your interest in contributing! This guide will help you get started.

## How to Contribute

### 1. Reporting Issues

Found a problem or have a suggestion? Please:

1. Check if the issue already exists in [Issues](https://github.com/hfourtech/geo_boilder_plate/issues)
2. If not, create a new issue with:
   - Clear title and description
   - Steps to reproduce (if applicable)
   - Expected vs actual behavior
   - Your environment details

### 2. Suggesting New Examples

We welcome new GEO optimization examples! When suggesting:

- Explain the use case
- Show why it's beneficial for GEO
- Provide working code examples
- Include schema markup where relevant

### 3. Submitting Pull Requests

#### Before You Start

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Make sure your contribution follows our guidelines

#### Code Standards

**For HTML Examples:**
- Use semantic HTML5 tags
- Include proper Schema.org markup
- Validate HTML with [W3C Validator](https://validator.w3.org/)
- Include inline comments explaining GEO-specific optimizations

**For Markdown Examples:**
- Use clear heading hierarchy (H1 → H2 → H3)
- Include code blocks with language tags
- Add tables for structured data
- Keep paragraphs concise and scannable

**For Schema Markup:**
- Validate JSON-LD with [Schema Validator](https://validator.schema.org/)
- Include all required properties
- Add comments explaining each field
- Use realistic example data

**For Code Examples:**
- Include comprehensive comments
- Show both "bad" and "good" examples
- Provide complete, runnable code
- Include error handling

#### Pull Request Process

1. Update the README.md if adding new examples
2. Add your example to the appropriate directory:
   - `/examples/html-structure/` - Semantic HTML examples
   - `/examples/schema-markup/` - Schema.org implementations
   - `/examples/api-docs/` - API documentation templates
   - `/examples/tutorials/` - Step-by-step guides
   - `/examples/benchmarks/` - Performance comparisons
3. Test your code/examples thoroughly
4. Write a clear PR description explaining:
   - What changes you made
   - Why these changes improve GEO
   - Any relevant screenshots or examples
5. Reference any related issues

### 4. Example Contribution Template

When adding a new example, include:

```markdown
# [Example Title]

## What This Optimizes
Brief explanation of the GEO benefit

## Use Case
When and why to use this pattern

## Code Example
[Your code with comments]

## Schema Markup
[Relevant schema if applicable]

## How AI Models Use This
Explanation of how this helps with citations

## Additional Resources
- Link 1
- Link 2
```

## Documentation Standards

### Writing Style

- **Clear and concise:** Avoid unnecessary jargon
- **Developer-focused:** Assume intermediate technical knowledge
- **Practical:** Include real-world examples
- **Accurate:** Cite sources and test claims

### Code Comments

Good comment example:
```javascript
/**
 * Generates a JWT token for user authentication
 * @param {Object} user - User object with id and email
 * @param {string} secret - Secret key for signing
 * @returns {string} Signed JWT token valid for 24 hours
 */
function generateToken(user, secret) {
  // Include only non-sensitive user info in payload
  // AI models can understand and cite this pattern
  const payload = {
    userId: user.id,
    email: user.email
  };
  
  return jwt.sign(payload, secret, { expiresIn: '24h' });
}
```

## Community Guidelines

### Be Respectful

- Welcome newcomers
- Provide constructive feedback
- Assume good intentions
- Focus on ideas, not individuals

### Share Knowledge

- Explain your reasoning
- Link to relevant resources
- Help others learn
- Celebrate contributions

### Stay On Topic

This repository focuses on:
- ✅ GEO optimization techniques
- ✅ Schema markup best practices
- ✅ Code examples for AI citation
- ✅ Technical documentation patterns

Please avoid:
- ❌ General SEO advice (unless directly related to GEO)
- ❌ Promotional content
- ❌ Off-topic discussions

## Getting Help

Need help with your contribution?

- **Questions:** Open a [Discussion](https://github.com/hfourtech/geo_boilder_plate/discussions)
- **Issues:** Check existing [Issues](https://github.com/hfourtech/geo_boilder_plate/issues)
- **Email:** your.email@example.com
- **Twitter:** [@yourhandle](https://twitter.com/yourhandle)

## Recognition

Contributors are recognized in:
- README.md acknowledgments section
- Release notes for significant contributions
- Our [Contributors page](https://github.com/hfourtech/geo_boilder_plate/graphs/contributors)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for helping make GEO optimization accessible to developers worldwide! 🙏