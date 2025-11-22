---
title: Ruby Setup Guide
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Ruby/Rails developers

## Description

Complete guide to setting up Forest Admin with Ruby on Rails using forest_liana gem.

## Sources to migrate

- `documentation-agent-v1/` Ruby setup guides
- Rails integration documentation

## Content outline

1. **Prerequisites**
   - Ruby version requirements (2.6+)
   - Rails version (5.2+)
   - Database setup
   - Forest Admin account

2. **Installation**
   ```ruby
   # Gemfile
   gem 'forest_liana'
   ```
   ```bash
   bundle install
   ```

3. **Rails Integration**
   - Initializer setup
   - Routes configuration
   - Middleware setup

4. **Database Connection**
   - ActiveRecord configuration
   - PostgreSQL
   - MySQL
   - SQLite (development only)

5. **Configuration**
   ```ruby
   # config/initializers/forest_liana.rb
   ForestLiana.env_secret = ENV['FOREST_ENV_SECRET']
   ForestLiana.auth_secret = ENV['FOREST_AUTH_SECRET']
   ```

6. **Environment Variables**
   - FOREST_ENV_SECRET
   - FOREST_AUTH_SECRET
   - DATABASE_URL
   - Rails environment setup

7. **Running Locally**
   - Rails server
   - Development mode
   - Debugging
   - Testing connection

8. **Project Structure**
   - lib/forest_liana/
   - Collection files
   - Action files
   - Rails conventions

9. **Next Steps**
   - Define Smart Actions
   - Add Smart Fields
   - Set up relationships
   - Deploy to production

## Migration notes

- Ruby PRIORITY 2
- Rails-specific patterns
- ActiveRecord integration
- Compare with Node.js version

## Related pages

- [Setup Overview](/product/integration/setup/overview)
- [Process > Actions > Ruby](/product/process/actions/coding-actions/ruby)
- [Guides > Deployment > Heroku](/guides/deployment/platforms/heroku)
