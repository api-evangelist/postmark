---
title: "Sending mission-critical emails in Python: SMTP or API?"
url: "https://postmarkapp.com/blog/sending-mission-critical-emails-in-python-smtp-or-api"
date: "2026-04-29T11:24:00-04:00"
author: "Greg Svoboda (gsvoboda@activecampaign.com)"
feed_url: "https://postmarkapp.com/blog/feed.atom"
---
<p>If you need to send emails from an application, it’s a good idea to think about how your usage changes over time. As with many technology choices, there’s a tradeoff between up-front efficiency and long-term flexibility.</p><p>You have two main choices for sending emails through your Python application. The first is SMTP — the standard protocol for outgoing email. The second option sidesteps SMTP's complexities by using an email provider’s API for richer functionality.</p><p>This article compares&nbsp;<a href="https://postmarkapp.com/send-email/python">sending email in Python</a> using SMTP and API. SMTP is quick to set up—especially with a library like smtplib—but it won't scale as your app grows, puts you at risk of spam flags, and makes diagnosing problems complex. An API setup takes a little longer, but it yields dividends with features you can leverage in many ways.</p><h2>Go lean with SMTP</h2><p>SMTP is a basic, universal protocol for&nbsp;<a href="https://postmarkapp.com/smtp-service">sending email</a>. Direct SMTP implementation is as close as you can get to DIY email, but even the&nbsp;<a href="https://www.youtube.com/watch?v=xB6u9zeRMpY">simplest sending setup</a> still requires a third-party SMTP server.&nbsp;</p><p>Technically speaking, you could&nbsp;<a href="https://postmarkapp.com/guides/everything-you-need-to-know-about-smtp">host your own SMTP server</a>, but it’s rarely a good tradeoff. Inbox providers pay close attention to&nbsp;<a href="https://postmarkapp.com/blog/how-to-check-your-ip-reputation">sender reputation</a> to determine which emails make it through. An established SMTP service helps ensure your emails are actually delivered. The code below uses the Postmark SMTP servers.</p><h3>Start fast with common tools (like smtplib)</h3><p><a href="https://postmarkapp.com/developer/user-guide/send-email-with-smtp">Sending email with SMTP</a> is straightforward and very similar in any application. Once set up, the advantage is that you can try a new service without completely overhauling your application — you can change SMTP service providers using the same code.</p><p>To implement SMTP in a Python application, use the built-in library called <a href="https://docs.python.org/3/library/smtplib.html">smtplib</a> and follow these steps:</p><p><strong>1. Import smtplib</strong></p><p>You don’t need to install any new libraries — the SMTP module is included when you install Python. Just import it, along with MIMEText, which helps with email formatting:</p>

      
    
      
        <pre><code class="language-bash">import smtplib
from email.mime.text import MIMEText</code></pre>

      
    
      
        <p><strong>2. Set up your SMTP server</strong></p><p>The structure of this section of code stays the same for all SMTP servers and accounts, but you must update the details and credentials:</p>

      
    
      
        <pre><code class="language-markup">SMTP_SERVER = &quot;smtp.postmarkapp.com&quot;  # Replace with your SMTP server
SMTP_PORT = 587  # Update with your port. We recommend using a TLS-enabled port
SMTP_USER = &quot;string12345&quot;  # Your SMTP server username/token
SMTP_PASS = &quot;passwordstring98765&quot;  # Your SMTP server password
FROM_ADDR = “no-reply@mailservice.com” # Your sender email address</code></pre>

      
    
      
        <p>Though we haven’t done it in this example, it’s a good idea to store your credentials as environment variables so they’re not exposed in public code.</p><p><strong>3. Create an email message</strong></p><p>The variables above can be reused across multiple email functions. Each function should create an email object:</p>

      
    
      
        <pre><code class="language-markup">to_addr = &quot;recipient@example.com&quot;
subject = &quot;SMTP Test&quot;	
body = &quot;Hello world! This is an SMTP service test!&quot;
	
	# Create the email object
test_message = MIMEText(body, &quot;plain&quot;)  # &quot;plain&quot; means text-only email
test_message[&quot;Subject&quot;] = subject
test_message[&quot;To&quot;] = to_addr</code></pre>

      
    
      
        <p><strong>4. Send the email message</strong></p><p>Pull all the pieces together in your script:</p>

      
    
      
        <pre><code class="language-markup">with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
    	server.starttls()  # Secure connection
    	server.login(SMTP_USER, SMTP_PASS)
    	server.sendmail(FROM_ADDR, to_addr, test_message.as_string())

print(&quot;Email sent successfully!&quot;)</code></pre>

      
    
      
        <p>This script is adaptable and flexible, within the limits of SMTP itself.</p><h3>Prepare for growing pains</h3><p>SMTP is a fine place to start, but it will not scale readily as your application grows in size and complexity.&nbsp;</p><p>The “S” in SMTP stands for “simple,” but it could also stand for “self-serve.” Compared to using an API, direct SMTP implementations offer fewer features and less observability.&nbsp;</p><p>SMTP doesn’t offer automatic retry, message batching, or queueing, all of which can significantly improve your delivery rates. If delivery fails, it can’t help you discover the reason. You could build these features yourself, but it would require expertise across many aspects of application architecture and email functionality.&nbsp;</p><p>SMTP is a good technology to understand, and can be useful for simple applications. But for many developers, building a feature-rich application around SMTP is not worth the trouble.&nbsp;</p><h2>Access flexibility and features with an API</h2><p>The second route is to integrate with a reputable email service like Postmark via its API. You still leverage their SMTP servers, but you also gain access to structured tools, logs, and reports that make your app more flexible and invaluable as it scales.</p><p>An&nbsp;<a href="https://postmarkapp.com/developer/user-guide/send-email-with-api">email API</a> transforms requests from your application into instructions for a network of connected SMTP servers and other related services.&nbsp;</p><p>Imagine taking a load of packages to a business center, where a professional at the counter determines the most appropriate and reliable service for your needs. An API is more than a mail carrier&nbsp; —&nbsp; it’s&nbsp; a service bundler that unlocks functionality SMTP alone can’t deliver.</p><h3>SDKs speed up setup</h3><p>It’s certainly possible to&nbsp;<a href="https://realpython.com/api-integration-in-python/">call an API</a> directly from your application. In a modern Python app, you’re likely to use the&nbsp;<a href="https://pypi.org/project/requests/">requests</a> module. It’s a well-traveled path and takes only a little legwork to establish.</p><p>A code base filled with HTTP requests can get messy quickly, though, and it can be time-consuming to set up, especially if you intend to use it in multiple parts of your application.&nbsp;</p><p>SDKs speed up the setup process, help you discover features, and lower the risk of errors in your code. In Python,&nbsp;<a href="https://docs.python-guide.org/writing/style/">working with an SDK</a> often helps keep your code base more “<a href="https://docs.python-guide.org/writing/style/">Pythonic</a>” — cleaner, more concise, and more readable.&nbsp;</p><p>To use Postmark’s new Python SDK to send a single email, follow these steps:</p><p><strong>1. Install the Python SDK</strong></p><p>Postmark has SDKs available in many languages, some&nbsp;<a href="https://postmarkapp.com/developer/integration/official-libraries">officially supported</a> and others maintained by&nbsp;<a href="https://postmarkapp.com/developer/integration/community-libraries">the Postmark community</a>.&nbsp;</p><p>For a Python application, <a href="https://pypi.org/project/postmark-python/">download the package here</a> and set up your credentials.</p><p>Save your credentials and sender email address as environment variables.</p><p><strong>2. Import modules</strong></p><p>In the file where you intend to send an email, import the following modules, and load your environment variables:</p>

      
    
      
        <pre><code class="language-markup">import asyncio
import os

import postmark

try:
   from dotenv import load_dotenv

   load_dotenv()
except ImportError:
   pass

SENDER = os.environ[&quot;POSTMARK_SENDER_EMAIL&quot;]</code></pre>

      
    
      
        <p><strong>3. Create an email function</strong></p><p>APIs offer many more sending options than SMTP alone, like&nbsp;<a href="https://postmarkapp.com/developer/api/templates-api">templates</a>,&nbsp;<a href="https://postmarkapp.com/developer/api/bounce-api">bounce tracking</a>, and&nbsp;<a href="https://postmarkapp.com/developer/api/bulk-email">bulk messaging</a>. If you take this route, explore what’s available. This is the most basic email sending function:</p>

      
    
      
        <pre><code class="language-markup">async def main():
   async with postmark.ServerClient(os.environ[&quot;POSTMARK_SERVER_TOKEN&quot;]) as client:
 
response = await client.outbound.send(
           postmark.Email(
               sender=SENDER,
               to=&quot;receiver@example.com&quot;,
               subject=&quot;SMTP Test&quot;,
               text_body=&quot;Hello world! This is a test of the Python API.&quot;,
               metadata={&quot;user_id&quot;: &quot;12345&quot;},
             )
           )
       	print(f&quot;Sent (model): {response.message_id}&quot;)

asyncio.run(main())</code></pre>

      
    
      
        <p>So far, we’ve mostly done what we did with smtplib above. But using the API offers two key advantages.&nbsp;</p><p>First, the built-in Email class ensures your messages are properly formatted, increasing the likelihood of successful delivery. Second, an API gives you access to response codes.</p><p><strong>4. Get confirmation</strong></p><p>One of the most valuable reasons to use an API is observability — you get much more information about what happens after you send an email.&nbsp;</p><p>The sending method above is wrapped in an async function that returns a response from the API. If your email sends correctly, the response looks like this:</p>

      
    
      
        <pre><code class="language-json">HTTP/1.1 200 OK
Content-Type: application/json

{
	&quot;To&quot;: &quot;receiver@example.com&quot;,
	&quot;SubmittedAt&quot;: &quot;2014-02-17T07:25:01.4178645-05:00&quot;,
	&quot;MessageID&quot;: &quot;0a129aee-e1cd-480d-b08d-4f48548ff48d&quot;,
	&quot;ErrorCode&quot;: 0,
	&quot;Message&quot;: &quot;OK&quot;
}</code></pre>

      
    
      
        <p>Even with this very basic level of tracking, you already have more insight into what happened to your email than with SMTP.&nbsp;</p><p>Other fields in the <a href="https://postmarkapp.com/developer/api/email-api">Email</a> class provide even more detail, like whether the recipient opened the email or clicked links — you can get this information by sending the MessageID to the <a href="https://postmarkapp.com/developer/api/messages-api">Messages</a> endpoint. The <a href="https://postmarkapp.com/developer/api/stats-api">Stats</a> endpoint aggregates details across all your sent messages so you can see trends.</p><h3>Build scalable, adaptable email functionality</h3><p>Setting up an API is definitely more work than setting up SMTP directly, but the payoff for your time is much greater.&nbsp;</p><p>An API can give you access to features like bulk sending and scheduled sending,&nbsp;<a href="https://postmarkapp.com/developer/webhooks/webhooks-overview">webhooks</a> for monitoring behavior and errors, and automatic retry — none of which are available with SMTP alone. Features like these allow you to build a more robust email program that can develop alongside your application.&nbsp;</p><h2>Re-evaluate as you grow</h2><p>Choosing the best way to implement email means keeping an eye on your needs as they evolve. Your application is likely to grow over time, so pay attention to your changing use cases and don’t stick to a method that no longer works for you.</p><p>If you’re not quite ready to invest in setting up an API, SMTP can be a fine place to start. But you can migrate email sending methods anytime, whether you’re thinking of switching from SMTP to an API or&nbsp;<a href="https://postmarkapp.com/migration-guides/">switching email service providers</a>. It may be much less of a challenge than you expect, and the benefits may be significant.&nbsp;</p>
