# veb

Headless browser automation CLI for agents and humans. Full Playwright parity.

## Installation

```bash
pnpm install
npx playwright install chromium
pnpm build
```

## Usage

```bash
# Navigation
veb open https://example.com
veb back                         # Go back
veb forward                      # Go forward  
veb reload                       # Reload page

# Page info
veb url                          # Get current URL
veb title                        # Get page title

# Clicking
veb click "#submit-btn"
veb dblclick "#item"

# Form input
veb type "#search" "query"           # Type character by character
veb fill "#email" "test@example.com" # Clear and fill (faster)
veb check "#agree"                   # Check checkbox/radio
veb uncheck "#newsletter"            # Uncheck checkbox
veb select "#country" "US"           # Select dropdown

# Keyboard
veb press Enter
veb press Tab

# Mouse
veb hover "#menu"
veb focus "#input"
veb drag "#source" "#target"

# File upload
veb upload "#file-input" ./document.pdf
veb upload "#files" ./a.png ./b.png

# Downloads
veb download "#download-btn" ./file.zip

# Waiting
veb wait "#loading"              # Wait for selector
veb wait --text "Welcome"        # Wait for text
veb wait 2000                    # Wait 2 seconds

# Screenshots & PDF
veb screenshot page.png
veb screenshot --full page.png   # Full page
veb screenshot -s "#hero"        # Specific element
veb pdf report.pdf

# Content extraction
veb snapshot                     # Accessibility tree (best for agents)
veb extract "#main"              # Get HTML
veb eval "document.title"        # Run JavaScript

# Element info
veb gettext "#message"           # Get text content
veb getattr "#link" "href"       # Get attribute
veb isvisible "#modal"           # Check visibility
veb isenabled "#submit"          # Check if enabled
veb ischecked "#checkbox"        # Check if checked
veb count ".items"               # Count matching elements
veb boundingbox "#element"       # Get position/size

# Scrolling
veb scroll down 500
veb scroll up
veb scroll -s "#container" down

# Semantic locators (Playwright's recommended approach)
veb role button click --name "Submit"
veb role textbox fill "hello" --name "Email"
veb text "Sign In" click
veb text "Submit" click --exact
veb label "Email" fill "test@test.com"
veb placeholder "Search..." fill "query"

# Frames/iframes
veb frame "#iframe"              # Switch to iframe
veb mainframe                    # Switch back to main

# Network interception
veb route "**/*.png" --abort                    # Block images
veb route "**/api/*" --body '{"mock":true}'     # Mock API
veb unroute                                      # Remove all routes
veb requests                                     # View tracked requests
veb requests --filter "api"                      # Filter requests

# Browser settings
veb viewport 1920 1080           # Set viewport size
veb device "iPhone 14"           # Emulate device
veb geolocation 37.7749 -122.4194  # Set location (SF)
veb permissions grant geolocation notifications

# Cookies
veb cookies                      # Get all cookies
veb cookies set '[{"name":"session","value":"abc123","domain":".example.com"}]'
veb cookies clear

# Storage
veb storage local                # Get all localStorage
veb storage local myKey          # Get specific key
veb storage local set key value  # Set value
veb storage local clear          # Clear localStorage
veb storage session              # sessionStorage (same commands)

# Dialogs (alerts, confirms, prompts)
veb dialog accept                # Accept next dialog
veb dialog accept "input text"   # Accept prompt with text
veb dialog dismiss               # Dismiss next dialog

# Tabs
veb tab new
veb tab list
veb tab 0                        # Switch to tab
veb tab close

# Windows
veb window new

# Sessions (isolate multiple agents)
veb --session agent1 open example.com
veb --session agent2 open google.com
VEB_SESSION=agent1 veb click "#btn"
veb session list

# Close browser
veb close
```

## Agent Mode

Use `--json` flag for machine-readable output:

```bash
veb snapshot --json
veb eval "document.title" --json
veb isvisible "#modal" --json
```

## All Commands

### Navigation
| Command | Description |
|---------|-------------|
| `open <url>` | Navigate to URL |
| `back` | Go back |
| `forward` | Go forward |
| `reload` | Reload page |
| `url` | Get current URL |
| `title` | Get page title |

### Interaction
| Command | Description |
|---------|-------------|
| `click <selector>` | Click element |
| `dblclick <selector>` | Double-click |
| `type <selector> <text>` | Type text |
| `fill <selector> <value>` | Clear & fill |
| `press <key>` | Press key |
| `check <selector>` | Check checkbox |
| `uncheck <selector>` | Uncheck |
| `select <selector> <value>` | Select option |
| `hover <selector>` | Hover |
| `focus <selector>` | Focus |
| `drag <src> <target>` | Drag & drop |
| `upload <selector> <files>` | Upload files |
| `download <selector> <path>` | Download file |
| `scroll <dir> [amount]` | Scroll |

### Element Info
| Command | Description |
|---------|-------------|
| `gettext <selector>` | Get text content |
| `getattr <selector> <attr>` | Get attribute |
| `isvisible <selector>` | Check visibility |
| `isenabled <selector>` | Check enabled |
| `ischecked <selector>` | Check checked |
| `count <selector>` | Count elements |
| `boundingbox <selector>` | Get bounds |

### Semantic Locators
| Command | Description |
|---------|-------------|
| `role <role> <action>` | By ARIA role |
| `text <text> <action>` | By text |
| `label <label> <action>` | By label |
| `placeholder <ph> <action>` | By placeholder |

### Content & Screenshots
| Command | Description |
|---------|-------------|
| `screenshot [path]` | Screenshot |
| `pdf <path>` | Save as PDF |
| `snapshot` | Accessibility tree |
| `extract <selector>` | Get HTML |
| `eval <script>` | Run JavaScript |
| `wait <sel\|text\|ms>` | Wait |

### Network
| Command | Description |
|---------|-------------|
| `route <url> [options]` | Intercept requests |
| `unroute [url]` | Remove routes |
| `requests [--filter]` | View requests |

### Browser Settings
| Command | Description |
|---------|-------------|
| `viewport <w> <h>` | Set viewport |
| `device <name>` | Emulate device |
| `geolocation <lat> <lng>` | Set location |
| `permissions grant\|deny` | Set permissions |

### Browser State
| Command | Description |
|---------|-------------|
| `cookies` | Get cookies |
| `cookies set <json>` | Set cookies |
| `cookies clear` | Clear cookies |
| `storage local [key]` | localStorage |
| `storage session [key]` | sessionStorage |
| `dialog accept\|dismiss` | Handle dialogs |

### Frames & Tabs
| Command | Description |
|---------|-------------|
| `frame <selector>` | Switch to frame |
| `mainframe` | Back to main |
| `tab new` | New tab |
| `tab list` | List tabs |
| `tab <index>` | Switch tab |
| `tab close [index]` | Close tab |
| `window new` | New window |

### Session & Control
| Command | Description |
|---------|-------------|
| `session` | Show session |
| `session list` | List sessions |
| `close` | Close browser |

## Options

| Option | Description |
|--------|-------------|
| `--session <name>` | Use isolated session |
| `--json` | JSON output |
| `--full, -f` | Full page screenshot |
| `--selector, -s` | Target element |
| `--name, -n` | Locator name filter |
| `--exact` | Exact text match |
| `--text, -t` | Wait for text |
| `--abort` | Abort route |
| `--body` | Route response body |
| `--filter` | Filter requests |
| `--debug` | Debug output |

## Device Emulation

```bash
# Mobile devices
veb device "iPhone 14"
veb device "iPhone 14 Pro Max"
veb device "Pixel 7"
veb device "Galaxy S23"

# Tablets
veb device "iPad Pro 11"
veb device "Galaxy Tab S8"

# Desktop
veb viewport 1920 1080
veb viewport 2560 1440
```

## Sessions

Sessions allow multiple agents to use veb simultaneously without interfering:

```bash
# Using --session flag
veb --session agent1 open https://site-a.com
veb --session agent2 open https://site-b.com

# Using environment variable
export VEB_SESSION=agent1
veb open https://example.com

# List all running sessions
veb session list

# Close a specific session
veb --session agent1 close
```

## Selectors

veb supports all Playwright selectors:

```bash
# CSS
veb click "#id"
veb click ".class"
veb click "div.container > button"

# Text
veb click "text=Click me"

# XPath
veb click "xpath=//button[@type='submit']"

# Semantic (recommended)
veb role button click --name "Submit"
veb label "Email" fill "test@test.com"
```

## License

MIT
