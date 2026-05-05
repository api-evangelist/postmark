---
title: "How to keep your Postmark account secure: Best practices guide"
url: "https://postmarkapp.com/blog/how-to-keep-your-postmark-account-secure-best-practices-guide"
date: "2025-10-21T10:42:00-04:00"
author: "Postmark team (fdossetto+postmark@activecampaign.com)"
feed_url: "https://postmarkapp.com/blog/feed.atom"
---
<p>Your Postmark API tokens are the keys to your email kingdom. One exposed token can mean unauthorized access to your account, potential data breaches, and a whole lot of stress you don't need. The good news? Keeping your account secure doesn't have to be complicated.</p><p>This guide covers the essential practices that'll help you sleep better at night, knowing your Postmark credentials are locked down tight.</p>

      
    
      
    
      
        <h2><strong>Following Industry Standards: OWASP Guidelines</strong></h2><p>When it comes to web application security, you don't have to reinvent the wheel. The Open Web Application Security Project (OWASP) provides comprehensive, industry-standard guidance that's trusted by security professionals worldwide.</p><h3><strong>The OWASP Web Security Testing Guide</strong></h3><p>The<a href="https://github.com/OWASP/wstg/blob/master/document/0-Foreword/README.md">&nbsp;OWASP Web Security Testing Guide</a> is an essential resource for developers who want to build secure applications. While it covers much more than just secret management, several sections are particularly relevant to protecting your Postmark credentials:</p><p><strong>Key areas that apply to API security:</strong></p><ul><li><strong>Authentication Testing</strong> - Ensuring your API tokens are properly validated</li><li><strong>Configuration and Deployment Management Testing</strong> - Securing your deployment pipeline</li><li><strong>Data Validation Testing</strong> - Validating inputs that might expose credentials</li><li><strong>Error Handling</strong> - Ensuring error messages don't leak sensitive information</li></ul><h3><strong>Applying OWASP Principles to Your Email Infrastructure</strong></h3><p>Here's how OWASP best practices translate to protecting your Postmark setup:</p><p><strong>Secure Configuration Management:</strong> Follow OWASP's guidance on configuration security by keeping all sensitive Postmark credentials in dedicated secrets management tools, never in code repositories.</p><h3><strong>Why OWASP Matters for Email Security</strong></h3><p>Email infrastructure is often overlooked in security assessments, but it's a critical attack vector. A compromised email service can lead to:</p><ul><li>Account takeover attacks through password reset emails</li><li>Data exfiltration via email forwarding</li><li>Social engineering attacks using legitimate email channels</li><li>Compliance violations if sensitive data is transmitted insecurely</li></ul><p>By following OWASP guidelines alongside the Postmark-specific practices in this guide, you're building defense in depth that protects both your email infrastructure and your broader application.</p><h2><strong>Code repository security: Your first line of defense</strong></h2>

      
    
      
    
      
        <h3><strong>Store secrets in environment variables</strong></h3><p>Never hardcode your Postmark API tokens directly in your source files. We've all been tempted to do it "just for now" during development, but that's how accidents happen.</p><p><strong>Do this instead:</strong></p><p>JavaScript</p>

      
    
      
        <pre><code class="language-javascript">// ❌ Don&#039;t do this
const postmark = require(&#039;postmark&#039;)(&#039;your-api-token-here&#039;);

// ✅ Do this
const postmark = require(&#039;postmark&#039;)(process.env.POSTMARK_API_TOKEN);</code></pre>

      
    
      
        <h3><strong>Loading environment variables in Node.js</strong></h3><p><strong>For Node.js 20.6.0 and later:</strong> Modern Node.js versions have built-in support for&nbsp;.env files using the&nbsp;--env-file flag:</p><p>bash</p>

      
    
      
        <pre><code class="language-bash">node --env-file=.env your-app.js</code></pre>

      
    
      
        <p><strong>For earlier Node.js versions:</strong> Use the popular<a href="https://www.npmjs.com/package/dotenv">&nbsp;dotenv package</a>:</p>

      
    
      
        <pre><code class="language-bash">npm install dotenv</code></pre>

      
    
      
        <p>Then load it early in your application:</p>

      
    
      
        <pre><code class="language-javascript">require(&#039;dotenv&#039;).config();
// Now process.env.POSTMARK_API_TOKEN is available</code></pre>

      
    
      
        <p><strong>For local development, create a&nbsp;.env file in your project root:</strong></p>

      
    
      
        <pre><code class="language-bash">POSTMARK_API_TOKEN=your-token-here
POSTMARK_SERVER_TOKEN=your-server-token-here</code></pre>

      
    
      
        <p><strong>Pro tip:</strong> Add your&nbsp;.env file to&nbsp;.gitignore immediately. Future you will thank you.</p><h3><strong>Other language examples</strong></h3><p>The concept applies universally – here are quick examples:</p>

      
    
      
        <pre><code class="language-markup"># Python
import os
token = os.environ.get(&#039;POSTMARK_API_TOKEN&#039;)</code></pre>

      
    
      
        <pre><code class="language-ruby"># Ruby
token = ENV[&#039;POSTMARK_API_TOKEN&#039;]</code></pre>

      
    
      
        <pre><code class="language-markup">// Go
token := os.Getenv(&quot;POSTMARK_API_TOKEN&quot;)</code></pre>

      
    
      
        <h3><strong>Keep configuration files separate</strong></h3><p>Create dedicated configuration files for sensitive data and exclude them from version control. Your&nbsp;.gitignore should include:</p>

      
    
      
        <pre><code class="language-markup">.env
.env.local
.env.production
config/secrets.json
config/database.yml</code></pre>

      
    
      
        <h3><strong>Set up Git hooks that actually help</strong></h3><p>Pre-commit hooks can catch secrets before they make it into your repository. Tools like&nbsp;git-secrets or&nbsp;pre-commit can scan for common patterns:</p>

      
    
      
        <pre><code class="language-bash"># Install git-secrets
git secrets --install
git secrets --register-aws
git secrets --add &#039;POSTMARK_[A-Z_]+.*&#039;</code></pre>

      
    
      
        <h2><strong>Secret management tools: level up your security</strong></h2><h3><strong>Consult&nbsp; with security and platform teams</strong></h3><p>Before diving into specific tools, have a conversation with your security, platform, and DevOps teams. They likely have established security strategies, approved tools, and compliance requirements that your Postmark implementation should align with.</p><p>What might seem like a simple API token to you could be part of a broader security framework that includes:</p><ul><li>Company-wide secret rotation policies</li><li>Approved secret management platforms</li><li>Compliance requirements (SOC 2, GDPR, HIPAA, etc.)</li><li>Existing CI/CD security integrations</li><li>Audit logging requirements</li></ul><p><strong>Start by asking:</strong></p><ul><li>What secret management tools are already approved and supported?</li><li>Are there existing patterns for API credential management?</li><li>What are the compliance or audit requirements for email infrastructure?</li><li>How should we integrate with existing monitoring and alerting systems?</li></ul><p>This conversation upfront can save you from having to retrofit security measures later.</p><h3><strong>Use dedicated secret managers</strong></h3><p>If you're running production applications, consider using dedicated secret management services:</p><ul><li><strong>HashiCorp Vault</strong> for on-premises solutions</li><li><strong>AWS Secrets Manager</strong> if you're in the AWS ecosystem</li><li><strong>Azure Key Vault</strong> for Microsoft based Azure cloud applications</li><li><strong>Google Secret Manager</strong> for Google Cloud Platform</li></ul><p>These services provide secure storage, automatic rotation, and audit trails for your sensitive credentials.</p><h3><strong>Integrate with your CI/CD pipeline</strong></h3><p>Configure your deployment pipeline to inject secrets at build or deploy time:</p>

      
    
      
        <pre><code class="language-markup"># GitHub Actions example
- name: Deploy to production
  env:
    POSTMARK_API_TOKEN: ${{ secrets.POSTMARK_API_TOKEN }}
  run: npm run deploy</code></pre>

      
    
      
        <h3><strong>Local development made simple</strong></h3><p>Tools like&nbsp;<a href="https://direnv.net/">direnv</a> automatically load environment variables when you enter a project directory, making local development seamless without compromising security.</p><h2><strong>Detection and prevention: Catching problems early</strong></h2><h3><strong>Implement automated scanning</strong></h3><p>Use tools that scan for exposed credentials:</p><ul><li><a href="https://gitleaks.io/"><strong>GitLeaks</strong></a> - Scans git repositories for secrets</li><li><a href="https://trufflesecurity.com/trufflehog"><strong>TruffleHog</strong></a> - Searches through git history for high-entropy strings</li><li><a href="https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning"><strong>GitHub Secret Scanning</strong></a> - Automatically detects known secret formats</li></ul><h3><strong>Make code reviews your safety net</strong></h3><p>Train your team to spot hardcoded secrets during code reviews. Create a checklist that includes:</p><ul><li>Are there any hardcoded API tokens or passwords?</li><li>Are configuration files properly excluded from version control?</li><li>Are environment variables used for all sensitive data?</li></ul><h3><strong>Regular security audits</strong></h3><p>Schedule periodic scans of your existing codebases, especially older repositories that might predate your current security practices.</p><h2><strong>Development practices that keep you safe</strong></h2><h3><strong>Official libraries and repositories</strong></h3><p>Make sure you're only using official Postmark repositories and libraries:</p><ul><li><a href="https://postmarkapp.com/developer">API documentation</a></li><li><a href="https://postmarkapp.com/developer/integration/official-libraries">Official libraries and SDKs</a></li><li><a href="https://github.com/ActiveCampaign/postmark-mcp">Official Postmark MCP - Github</a></li><li><a href="https://postmarkapp.com/support">Support channels</a> or email <a href="mailto:security@activecampaign.com">security@activecampaign.com</a> if you have questions</li></ul><h3><strong>Follow the principle of least privilege</strong></h3><p>Use Postmark's server structure to isolate environments and control access.</p><p>Setup dedicated servers for development, staging and production. Each server will have a unique API key and only the production server will have full sending capabilities. This approach provides granular control over sending permissions while maintaining clear separation between environments</p><ul><li><strong>Development server:</strong> Dedicated server configured to use&nbsp;<a href="https://postmarkapp.com/developer/user-guide/sandbox-mode/server-sandbox-mode">sandbox mode</a></li><li><strong>Staging server:</strong> Mirror of production server configuration but also configured to use&nbsp;<a href="https://postmarkapp.com/developer/user-guide/sandbox-mode/server-sandbox-mode">sandbox mode</a></li><li><strong>Production server:</strong> Full sending capabilities with tightly controlled API token access</li></ul><h3><strong>Rotate your credentials regularly</strong></h3><p>Set up a schedule for rotating your Postmark API tokens:</p><ol><li>Generate new tokens in your Postmark account</li><li>Update them in your secret management system</li><li>Deploy the changes</li><li>Revoke the old tokens</li></ol><p><strong>Important:</strong> Always test the new tokens in a non-production environment first.</p><h3><strong>Create Template Configuration Files</strong></h3><p>Provide example configuration files that show the structure without actual secrets:</p>

      
    
      
        <pre><code class="language-markup">// config.example.json
{
  &quot;postmark&quot;: {
    &quot;serverToken&quot;: &quot;POSTMARK_SERVER_TOKEN_HERE&quot;,
    &quot;accountToken&quot;: &quot;POSTMARK_ACCOUNT_TOKEN_HERE&quot;
  },
  &quot;database&quot;: {
    &quot;host&quot;: &quot;DATABASE_HOST_HERE&quot;,
    &quot;password&quot;: &quot;DATABASE_PASSWORD_HERE&quot;
  }
}</code></pre>

      
    
      
        <p>This helps new team members understand what credentials they need without exposing real values.</p><h2><strong>When things go wrong: Remediation steps</strong></h2><p>Despite your best efforts, secrets sometimes get exposed. Here's your emergency playbook:</p><h3><strong>Step 1: Revoke the exposed keys immediately</strong></h3><p>This is your highest priority. Log into your Postmark account and:</p><ol><li>Navigate to your API tokens section</li><li>Revoke the compromised token</li><li>Generate a new token</li><li>Update your applications with the new token</li></ol><p><strong>Time matters here.</strong> Don't wait to investigate how it happened – secure first, investigate later.</p><h3><strong>Step 2: Remove from repository</strong></h3><p>Delete the secret from your current codebase and commit the change:</p>

      
    
      
        <pre><code class="language-bash">git add .
git commit -m &quot;Remove exposed API token&quot;
git push</code></pre>

      
    
      
        <p>Remember: The secret still exists in your git history.</p><h3><strong>Step 3: Clean up Git history (if necessary)</strong></h3><p>For public repositories or highly sensitive credentials, you may need to rewrite history:</p>

      
    
      
        <pre><code class="language-bash"># Using git filter-repo (recommended)
git filter-repo --replace-text secrets.txt

# Or using BFG Repo-Cleaner
java -jar bfg.jar --replace-text secrets.txt</code></pre>

      
    
      
    
      
        <h3><strong>Step 4: Coordinate with your team</strong></h3><p>If you need to rewrite history:</p><ol><li>Notify all team members</li><li>Have them back up any local changes</li><li>Force push the cleaned repository</li><li>Have team members re-clone</li></ol><h3><strong>Step 5: Use platform-specific tools</strong></h3><p>GitHub and GitLab offer built-in tools for handling exposed secrets:</p><ul><li><strong>GitHub:</strong> "Remove sensitive data" feature in repository settings</li><li><strong>GitLab:</strong> Repository cleanup options in the project settings</li></ul><p><strong>Proper Error Handling:</strong> When your application encounters Postmark API errors, log them securely without exposing your API tokens or other sensitive information in logs that might be accessible to unauthorized users.</p><p><strong>Regular Security Testing:</strong> Use OWASP's testing methodologies to regularly audit how your application handles Postmark credentials, especially during authentication failures or API rate limiting scenarios.</p><h2><strong>Making security the easy choice</strong></h2><p>The key to good security is making secure practices the path of least resistance. When it's easier to do the right thing than the wrong thing, your team will naturally make better choices.</p><p>Set up your development environment so that:</p><ul><li>Environment variables are the default way to handle configuration</li><li>Secrets are never committed to version control</li><li>New team members can get started quickly without compromising security</li></ul><h2><strong>Your security checklist</strong></h2><p>Here's a quick checklist to audit your current setup:</p>

      
    
      
    
      
    
      
    
      
        <h2><strong>The Bottom Line</strong></h2><p>Securing your Postmark account isn't about implementing every possible security measure – it's about building sustainable practices that protect you without slowing down your development process.</p><p>Start with the basics: environment variables, proper&nbsp;.gitignore configuration, and team awareness. As your application grows, add more sophisticated tools like secret managers and automated scanning.</p><p>Your users trust you with their email communications. These practices help ensure that trust is well-placed.</p><p><strong>Need help with Postmark security settings?</strong> Check out our<a href="https://postmarkapp.com/support/article/1183-best-practices-for-user-account-security"> security documentation</a> or reach out to our support team. We're here to help you keep your account secure.</p>
