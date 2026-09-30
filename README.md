# Matic production backlog

Two password-protected dashboards for the production team:

- **Matic/** (`/backlog/Matic/`) — unfulfilled Matic robot orders, aged into urgency zones
- **Hepa/** (`/backlog/Hepa/`) — outstanding HEPA bags by order date

## Why the files look like noise

Each page is **AES-256-GCM ciphertext**, with the key derived from the password by
PBKDF2-SHA256 at 310,000 iterations. The browser decrypts in memory after the password
is entered. There is no plaintext in this repo — a JavaScript password check would have
been meaningless here, since the page source is served before any check runs.

## Regenerating

Pages are generated in `~/Matic Scripts/Shopify`, then encrypted before committing:

    python3 matic-backlog-heat.py
    python3 hepa-backlog-heat.py
    python3 encrypt_page.py matic_backlog_heat.html Matic/index.html "<password>"
    python3 encrypt_page.py hepa_backlog_heat.html  Hepa/index.html  "<password>"

`encrypt_page.py` needs the `cryptography` package, which lives on the system Python
rather than the repo venv.

## Limits

Anyone with the password can open and save the decrypted page; this gates access, it
does not control redistribution. The password is shared, so there is no record of who
opened it.
