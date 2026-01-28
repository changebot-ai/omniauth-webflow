# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

OmniAuth strategy gem for authenticating with the Webflow API v2. Extends `omniauth-oauth2` to provide OAuth2 authentication flow for Webflow.

## Commands

```bash
bundle install                        # Install dependencies
gem build omniauth-webflow.gemspec    # Build the gem
bundle exec rake release              # Release to RubyGems
```

## Architecture

- `lib/omniauth-webflow.rb` - Entry point, requires version and strategy
- `lib/omniauth/strategies/webflow.rb` - Core OmniAuth strategy implementation
  - Configures OAuth2 endpoints (authorize at webflow.com, token at api.webflow.com)
  - Uses `/v2/token/introspect` endpoint to fetch user info via `raw_info`
  - Overrides `token_params` to include client credentials in token request
  - Overrides `callback_url` to use simple `full_host + callback_path` format
- `lib/omniauth-webflow/version.rb` - Version constant

## Webflow OAuth2 Flow

Endpoints:
- Authorization: `https://webflow.com/oauth/authorize`
- Token exchange: `https://api.webflow.com/oauth/access_token`
- User info: `https://api.webflow.com/v2/token/introspect`

Callback URL format: `https://[hostname]/auth/webflow/callback`
