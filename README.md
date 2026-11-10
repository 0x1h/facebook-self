facebook-self

async python wrapper for interacting with a logged-in facebook account through graphql.

features

cookie authentication

loads an existing facebook session from a browser cookie export. uses "c_user", "xs", and "datr" to authenticate requests without requiring a password or interactive login.

token extraction

fetches the facebook homepage and extracts the tokens required for authenticated requests:

- "fb_dtsg"
- "lsd"
- "jazoest"
- "rev"

calculates "jazoest" from "fb_dtsg" instead of scraping it independently.

session management

maintains a shared cookie jar and http session across requests. initializes the session through an async context manager and closes the http session when the context exits.

graphql request runner

constructs and sends authenticated graphql requests using the minimum verified request envelope. maintains request-specific parameters, including the "__req" counter.

response classification

parses facebook responses and converts application-level errors into specific exceptions.

- "NoSession" — session is missing or no longer authenticated.
- "Challenge" — facebook requires checkpoint verification.
- "Limited" — request was rate-limited; includes "wait" when available.
- "Stale" — request tokens have expired or become invalid.
- "Rotated" — an operation references an unknown document id.
- "Unknown" — unrecognized error code; preserves the original code and response body.

viewer query

provides "Client.viewer()" to retrieve the authenticated account's viewer data through graphql.

operation registry

stores graphql operation names and their corresponding document ids in "fbself/graphql/registry.py".

currently verified operations:

- "CometSettingsBadgeQuery"
- "CometLogoutHandlerQuery"
- "ScreenTimeInit_AggregatedScreenTimeQuery"

unverified operations raise "KeyError" rather than sending requests with missing or placeholder document ids.

planned features

- messenger — graphql operations for retrieving and interacting with messenger conversations.
- feed — graphql operations for accessing feed data.
- profiles — graphql operations for retrieving profile information.
- message polling — asynchronous polling through the "/pull" endpoint.
- html fallback — alternative response handling when graphql operations are unavailable.

requirements

- python 3.10+
- "curl_cffi"
- "cryptography"

license

mit