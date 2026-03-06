# WP-Members User Verification (Action Network)

A WordPress plugin that verifies newly registered users against an Action Network subscriber list and automatically populates their profile with membership data.

## Features

- Automatically schedules a verification check 10 seconds after user registration (via WP-Cron)
- Matches users by AFSCME ID (preferred) or last-4 SSN against Action Network custom fields
- On successful match: populates name, phone, address, member status, bargaining unit, and other union-specific meta fields
- Auto-patches malformed SSNs in Action Network (e.g. `123` → `0123`) before matching
- Manual sync button on each user's admin profile page
- Bulk action on the Users list to queue multiple users for sync
- "Needs Verification" filter view on the Users list
- Sortable "Member Status" column on the Users list

## Requirements

- WordPress 5.0+
- PHP 7.4+
- WP-Members plugin
- Action Network API key

## Installation

1. Upload the `action-network-user-verification` folder to `/wp-content/plugins/`
2. Activate the plugin through the Plugins menu in WordPress
3. Go to Settings and add your Action Network API key via the `action_network_api_key` option (can be set with `update_option()` or a settings page)

## Configuration

The plugin reads one WordPress option:

| Option | Description |
|--------|-------------|
| `action_network_api_key` | Your Action Network OSDI API token |

## User Meta Fields Written

On successful verification, the following user meta fields are populated from Action Network (only if currently empty):

`first_name`, `last_name`, `phone1`, `billing_address_1`, `billing_city`, `billing_state`, `billing_postcode`, `birthday`, `member_status`, `island`, `unit_number`, `job_title`, `baseyard`, `jurisdiction`, `employer`, `bargaining_unit`, `afscme_id`

Verification status is tracked via:

| Meta Key | Value |
|----------|-------|
| `an_verified` | `true` / `false` |
| `an_verification_note` | Human-readable result message |
| `an_last_updated_fields` | Comma-separated list of fields updated |

## Changelog

### 1.8
- Added ABSPATH security guard
- Fixed XSS: escape `$note` and `$fields` in admin sync result notice
- Standardized plugin header format
- Added README.md

### 1.7
- Added AFSCME ID matching as primary verification method
- SSN matching retained as fallback

### Earlier
- Initial verification via SSN matching against Action Network
