# Postmark GraphQL Schema

## Overview

This document describes a conceptual GraphQL schema for the Postmark transactional email API. Postmark provides a REST API for sending and tracking transactional and broadcast email, managing templates, servers, bounces, suppressions, statistics, webhooks, and inbound message processing.

Official API reference: https://postmarkapp.com/developer/api/overview

## Schema Design

The schema covers all major functional areas of the Postmark API:

- **Messages** — sending email, retrieving message details and delivery status
- **Templates** — managing reusable email templates and layouts
- **Bounces** — tracking bounced messages, bounce types, and suppression
- **Suppressions** — managing suppression lists for deliverability
- **Opens and Clicks** — engagement tracking events
- **Delivery** — delivery event details and confirmation
- **Stats** — aggregate delivery and engagement statistics
- **Servers** — account server management and configuration
- **Domains** — domain authentication (SPF, DKIM, Return-Path)
- **Signatures** — sender signatures and verification
- **Inbound** — inbound email parsing and routing rules
- **Webhooks** — event webhook configuration
- **API Keys and Tokens** — authentication credential management

## Types Summary

| Type | Description |
|------|-------------|
| Message | Top-level sent email message |
| MessageDetails | Full metadata for a sent message |
| MessageStatus | Delivery status enumeration |
| MessageType | Transactional vs broadcast type |
| EmailMessage | Input object for sending email |
| EmailDetails | Detailed email content fields |
| EmailHeader | Custom email header key-value pair |
| EmailAttachment | Attachment with content and metadata |
| TemplatedEmail | Input for sending via template |
| Template | Reusable email template |
| TemplateDetails | Full template content and metadata |
| TemplateAlias | Human-readable alias for a template |
| TemplateModel | Key-value model for template rendering |
| Layout | Template layout wrapper |
| LayoutAlias | Human-readable alias for a layout |
| LayoutDetails | Full layout content and metadata |
| Bounce | Record of a bounced message |
| BounceDetails | Extended bounce information |
| BounceType | Enumeration of bounce categories |
| BounceStatus | Current processing status of a bounce |
| Suppression | Suppressed email address record |
| SuppressionDetails | Extended suppression record |
| SuppressionType | Reason category for suppression |
| SuppressionOrigin | How the suppression was created |
| Open | Email open tracking event |
| OpenDetails | Extended open event metadata |
| OpenClient | Email client used at open |
| OpenOS | Operating system at open |
| OpenUserAgent | Raw user agent at open |
| Click | Link click tracking event |
| ClickDetails | Extended click event metadata |
| ClickLink | The clicked URL details |
| Delivery | Successful delivery event |
| DeliveryDetails | Extended delivery event metadata |
| Stats | Aggregate statistics summary |
| StatDetails | Individual stat metric details |
| StatPeriod | Time period for statistics |
| Server | Postmark server (environment) |
| ServerDetails | Extended server configuration |
| ServerType | Server plan/type enumeration |
| Domain | Domain used for sending |
| DomainDetails | Full domain record with DNS info |
| SPF | SPF DNS record status |
| DKIM | DKIM signing key status |
| ReturnPath | Return-Path address configuration |
| Signature | Sender signature record |
| SenderDetails | Verified sender email details |
| Sender | Sender identity for a message |
| InboundMessage | An email received via inbound processing |
| InboundDetails | Full parsed inbound email details |
| ParsedEmail | Structured parsed inbound email |
| InboundRule | Inbound routing rule |
| InboundFilter | Criteria for filtering inbound mail |
| APIKey | Account-level API key |
| Token | Server-level authentication token |
| WebhookTrigger | Event type that fires a webhook |
| Webhook | Webhook endpoint configuration |

## File

- `postmark-schema.graphql` — Full GraphQL SDL schema with 60+ named types
