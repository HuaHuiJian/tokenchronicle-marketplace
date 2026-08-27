# TokenChronicle Feedback Receiver Protocol v2

A clean source release ships no receiver URL, invitation, installation credential, account, or cloud
configuration. A publisher build may embed one public receiver URL in `tokenchronicle.publisher`; this is
public routing configuration, not a credential. Plain HTTP is accepted only for loopback development
addresses.

The production URL must use the exact form `https://<feedback-host>/v1/feedback`: standard HTTPS port,
no embedded credentials, query string, fragment, or redirect. The activation URL is derived as
`https://<feedback-host>/v1/installations/activate`. A stable custom domain should be used instead of
a cloud-provider function URL so the backend can move without upgrading clients.

## Activation

The publisher creates a short-lived, one-time invitation through the private administration API:

```http
POST /v1/admin/invitations
Authorization: Bearer <ADMIN_TOKEN>
Content-Type: application/json

{"valid_days":7}
```

The client exchanges that invitation once at `POST /v1/installations/activate`. The receiver returns
`installation_id` and `installation_token` only once; it stores only the token's SHA-256 hash. The
client keeps the token in private user-local configuration and never prints it. Invitations expire
within 30 days and become invalid immediately after activation.

## Feedback request

```http
POST /v1/feedback
Authorization: Bearer tc1_<installation-id>_<secret>
Idempotency-Key: <16-128 URL-safe characters>
Content-Type: application/json
```

The UTF-8 JSON object may contain only product/schema version, creation time, category, optional
rating, source page, user-written message, and the explicit privacy declaration. Conversation data
must be declared absent and network transmission explicitly confirmed.

Receivers reject unknown fields, requests larger than 32 KiB, missing or revoked credentials, and
missing idempotency keys. Repeating the same key for the same installation returns the original receipt
without creating another record.

## Response

```json
{
  "feedback_id": "tcf_<time>_<random-id>",
  "received_at": "2026-08-11T00:00:00Z",
  "duplicate": false
}
```

TokenChronicle stores only the status, HTTP status, bounded receipt ID, duplicate flag, and received time.
Receiver response bodies are not preserved. Redirects are rejected across trust domains.

## Security contract

- Installation credentials grant only feedback write access and are independently revocable.
- Administration endpoints require a private access layer; `ADMIN_TOKEN` is bootstrap authentication,
  not sufficient Internet-facing administrator authentication.
- WAF or a current cloud-native gateway must enforce IP, credential, and global limits before
  serverless compute.
- Application storage does not persist IP, User-Agent, project path, thread ID, conversation, archive,
  device fingerprint, or cloud credential.
- Operators revoke one installation with `PATCH /v1/admin/installations/<id>` and
  `{"status":"revoked"}`. Revocation takes effect on the next request.
- Accepted feedback is retained indefinitely by default. `RETENTION_DAYS=0` means persistent storage.
  A protected `POST /v1/admin/maintenance/purge` endpoint deletes nothing in this mode; it performs
  bounded cleanup only when the operator explicitly configures a positive retention period.
- Deployment packages contain synthetic tests only and are blocked if user paths, thread IDs, or
  embedded credentials are detected.
