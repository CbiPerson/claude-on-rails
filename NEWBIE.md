# Getting Started with ClaudeOnRails

A hands-on guide to demoing and using ClaudeOnRails for the first time.

**See it in action:** The [CLAUDE-ON-RAILS-DEMO](https://github.com/CbiPerson/CLAUDE-ON-RAILS-DEMO) repo is a complete working example built by following these steps.

## What You'll Need

Before you start, make sure you have:

- **Ruby 3.3+** - Check with `ruby -v`
- **Rails 6.0+** - Check with `rails -v`
- **Claude Code CLI** - Install from https://docs.anthropic.com/en/docs/claude-code
- **A Bundler install** - Check with `bundler -v` (comes with Ruby)

You do **not** need to install `claude-swarm` separately -- it's pulled in as a dependency automatically.

## Step 1: Create a Rails App

The fastest way to get a demo app is with the [swarmpod-core](https://github.com/CbiPerson/swarmpod-core) template:

```bash
gem install swarmpod-core
rails new demo_app --template $(gem contents swarmpod-core | grep rails_swarmpod.rb)
cd demo_app
```

Or clone the template repo directly:

```bash
git clone https://github.com/CbiPerson/swarmpod-core.git /tmp/swarmpod-core
rails new demo_app --template /tmp/swarmpod-core/templates/rails_swarmpod.rb
cd demo_app
```

You can also use any existing Rails project, or create a plain one:

```bash
rails new demo_app
cd demo_app
```

## Step 2: Add ClaudeOnRails

Add the gem to your Rails project's Gemfile:

```ruby
# Gemfile
group :development do
  gem 'claude-on-rails'
end
```

Then install:

```bash
bundle install
```

## Step 3: Generate the Swarm Configuration

Run the generator from your Rails project directory:

```bash
rails generate claude_on_rails:swarm
```

This will:

1. **Analyze your project** - Detects your database, test framework, JavaScript bundler, installed gems, and app patterns.
2. **Ask about Rails MCP Server** - Press **Y** to enable real-time Rails documentation access for agents (recommended).
3. **Generate files** - Creates `claude-swarm.yml`, updates `CLAUDE.md`, and creates `.claude-on-rails/` with agent prompts.

You'll see output showing exactly what was detected and created.

## Step 4: Review What Was Generated

Take a look at the key files:

```bash
# The swarm configuration -- this defines your AI team
cat claude-swarm.yml

# Agent prompts -- each agent has specialized instructions
ls .claude-on-rails/prompts/
```

The `claude-swarm.yml` file defines agents like Architect, Models, Controllers, Views, Services, Tests, and DevOps. Each has a specific directory scope and set of connections to other agents.

## Step 5: Start the Swarm

```bash
claude-swarm
```

This opens Claude Code with the swarm loaded. You'll be talking to the **Architect** agent, which coordinates the rest of the team.

## Step 6: Try It Out

Once the swarm is running, type natural language prompts. Here are good first demos:

### Simple feature (good for a quick demo)

```
> Create a Post model with title and body, a PostsController with full CRUD, and views to match
```

### Something more impressive

```
> Build user authentication with email/password signup, login, logout, and password reset
```

### For an API-only app

```
> Create a RESTful API for a task management system with users, projects, and tasks
```

The `>` prefix means you're typing in the Claude interface, not a terminal.

## What Happens Behind the Scenes

When you describe a feature:

1. The **Architect** analyzes your request and breaks it into tasks
2. It delegates to specialists -- **Models** for database/ActiveRecord, **Controllers** for routing/actions, **Views** for templates, etc.
3. Each agent works in its own directory scope and follows Rails conventions
4. The **Tests** agent adds test coverage
5. Everything is coordinated automatically

## Common Generator Options

```bash
# API-only app (no Views agent, adds API agent instead)
rails generate claude_on_rails:swarm --api-only

# Include GraphQL support
rails generate claude_on_rails:swarm --graphql

# Skip the test agent
rails generate claude_on_rails:swarm --skip-tests

# Skip MCP server setup
rails generate claude_on_rails:swarm --no-mcp-server
```

## If Something Goes Wrong

**`claude-swarm` command not found:**
Make sure the gem installed correctly. Run `bundle exec claude-swarm` instead, or check that your gem bin directory is in your PATH.

**Generator fails:**
Make sure you're running from the root of a Rails project (there should be a `config/application.rb` file).

**MCP setup issues:**
You can skip it during generation and set it up later:
```bash
bundle exec rake claude_on_rails:setup_mcp
```

Check status anytime with:
```bash
bundle exec rake claude_on_rails:mcp_status
```

**Want to regenerate:**
The generator is idempotent -- running it again will regenerate the configuration safely.

## Customizing Your Setup

After the initial setup, you can customize:

- **Agent prompts** - Edit files in `.claude-on-rails/prompts/` to add project-specific conventions or domain knowledge
- **Swarm config** - Edit `claude-swarm.yml` to add custom agents, change model assignments, or adjust agent connections
- **CLAUDE.md** - Add project-level instructions that all agents will follow

## Related Repos

- [claude-on-rails](https://github.com/CbiPerson/claude-on-rails) -- the gem itself
- [CLAUDE-ON-RAILS-DEMO](https://github.com/CbiPerson/CLAUDE-ON-RAILS-DEMO) -- a working demo built from these steps
- [swarmpod-core](https://github.com/CbiPerson/swarmpod-core) -- the Rails template used as a starting point

## Next Steps

- Read the [full README](./README.md) for feature details
- Check [examples](./examples/README.md) for more workflow scenarios
- See [SETUP.md](./SETUP.md) for advanced configuration and troubleshooting
- Visit [CONTRIBUTING.md](./CONTRIBUTING.md) if you want to contribute
