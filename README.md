# PHP Error Catcher & Diagnoser

A WordPress admin tool that captures, stores, and explains PHP errors in your site. It records errors, exceptions, and fatal shutdowns, shows rich details (stack traces and request context), provides built‑in quick diagnoses, and can optionally send the error to Xon AI for an intelligent diagnosis with a modern shimmering loader and typing‑effect UI.

## Features

- Catch PHP errors, exceptions, and fatal shutdowns early in WordPress execution
- Store logs in a dedicated database table for review (latest 100 displayed)
- View details: message, file, line, URL, stack trace, and request context
- Quick built‑in diagnoses for common error patterns
- Xon AI powered diagnosis:
  - One‑click “Diagnose with Xon AI” for each error
  - Shimmering loader while the request is in flight
  - Typewriter effect for streaming‑like result rendering
- Manage logs: delete individual entries or clear all
- “Trigger Test Error” button for easy verification

## Requirements

- WordPress 5.6+ (recommended)
- PHP 7.4+ (PHP 8.x compatible)
- Outbound HTTPS requests enabled (for Xon AI)

## Installation

1. Copy the plugin folder `php-error-catcher` into `wp-content/plugins/`.
2. In wp-admin, go to Plugins → “PHP Error Catcher & Diagnoser” → Activate.
3. In the admin menu, open “PHP Errors” to access the dashboard.
4. Optional (for AI): Configure your Xon AI key in the “Xon AI Settings” card at the top of the page.

## Xon AI Integration

This plugin integrates with Xon AI to generate intelligent diagnoses for captured errors.

- Endpoint: `POST https://xon-ai-zeta.vercel.app/api/platform`
- Payload:

```json
{
  "prompt": "Diagnose this PHP error and provide suggestions to fix it: ...",
  "unique_key": "YOUR_KEY_HERE"
}
```

### Getting a Key

- Purchase/obtain your `unique_key` at: https://xon-ai-zeta.vercel.app/

### Configuring the Key

1. Go to wp-admin → PHP Errors.
2. In “Xon AI Settings”, paste your `unique_key` and click “Save Settings”.
3. Expand any error, click “Diagnose with Xon AI” to get an AI‑generated explanation and fix suggestions.

### Response Handling

- The plugin supports responses in OpenAI-style format (e.g., `choices[0].message.content`) and a simpler `{ "response": "..." }` shape.
- Any `<think>...</think>` content returned by the model is stripped before display.

## Using the Dashboard

- “Trigger Test Error” simulates a warning so you can test logging and AI diagnosis.
- Click “Details” for any error to see stack trace, context, built‑in diagnosis, and the “Diagnose with Xon AI” button.
- Use “Delete” to remove a single log entry or “Clear All Logs” to truncate the table.

## How It Works

- A custom table (prefixed with your site’s `$wpdb->prefix`) stores error logs with metadata (type, message, file, line, stack trace, context, URL, user, timestamp).
- Error capture hooks:
  - `set_error_handler` for PHP errors
  - `set_exception_handler` for uncaught exceptions
  - `register_shutdown_function` for fatal errors
- Admin UI:
  - Menu: “PHP Errors”
  - Rich details and actionable suggestions
  - AJAX endpoint for Xon AI requests with nonce and capability checks

## Security & Privacy

- Admin-only access (capability check `manage_options`)
- Nonce protection on destructive actions and AJAX requests
- Key is stored in WordPress options; never logged in error entries
- Data sent to Xon AI includes error message, file, line, and stack trace; avoid using AI diagnosis on sensitive environments if necessary

## Troubleshooting

- “Xon AI Unique Key not set”: Save your key in the “Xon AI Settings” card.
- “API request failed”: Check server outbound connectivity and SSL. Confirm that WordPress can make remote requests.
- “Unexpected response format from AI”: The endpoint may have changed; ensure the key is valid and try again.
- No logs visible: Trigger a test error and refresh the page.

## Contributing

Pull requests and issue reports are welcome. Consider:

- Keeping UI changes consistent with WordPress admin styles
- Preserving capability and nonce checks for security
- Not logging secrets or sensitive information

## License

GPL-2.0-or-later

## Author

- Author: Omotayo Sonde  
- Author URL: https://my-portfolio-xs.vercel.app/

