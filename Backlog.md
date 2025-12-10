# Agile Backlog

**Generated**: 2025-12-10T07:14:59.689085
**Total Epics**: 6
**Total Stories**: 18
**Total Points**: 107
**Estimation Scale**: fibonacci
**SSCS Compliance**: v2.0
**TDD Workflow**: RED → GREEN → REFACTOR
**Branch Naming**: `feature/{issue-number}-{slug}`


## Epic #1000: Authentication & User Management

Implement secure JWT-based authentication system with role-based access control for DJs and admins, including registration, login, token refresh, and password reset functionality.

### User Stories

#### Story #1001: As a DJ, I want to register an account so that I can access the streaming platform

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement user registration endpoint with email/password validation, secure password hashing, and JWT token generation. Store user credentials in PostgreSQL with encrypted sensitive fields.

**Acceptance Criteria**:
1. Given a new user with valid email and password, When they submit registration form, Then account is created with hashed password and JWT tokens are returned
2. Given a user with invalid email format, When they attempt registration, Then a 400 Bad Request error is returned with validation details
3. Given a user with existing email, When they attempt registration, Then a 409 Conflict error is returned
4. Given a registered user, When they receive JWT tokens, Then access token expires in 24h and refresh token is stored securely

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for user registration endpoint`
- 🟢 GREEN: `green: user registration with JWT and password hashing`
- 🔵 REFACTOR: `refactor: extract validation utilities and improve error handling`

**Branch**: `feature/1001-user-registration`

**Labels**: user-story, feature, authentication

#### Story #1002: As a DJ, I want to login to my account so that I can manage my streams and content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement secure login endpoint with email/password authentication, JWT token generation, and refresh token mechanism. Include rate limiting to prevent brute force attacks.

**Acceptance Criteria**:
1. Given a registered user with correct credentials, When they submit login form, Then JWT access and refresh tokens are returned
2. Given a user with incorrect password, When they attempt login, Then a 401 Unauthorized error is returned
3. Given a user with non-existent email, When they attempt login, Then a 401 Unauthorized error is returned
4. Given a user with valid refresh token, When they request token refresh, Then a new access token is returned
5. Given excessive failed login attempts, When threshold is exceeded, Then account is temporarily locked with CAPTCHA challenge

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for login and token refresh`
- 🟢 GREEN: `green: JWT authentication with refresh token flow`
- 🔵 REFACTOR: `refactor: add rate limiting and improve security measures`

**Branch**: `feature/1002-user-login`

**Labels**: user-story, feature, authentication

#### Story #1003: As a DJ, I want to reset my password so that I can regain access if I forget it

**Points**: 3 | **Type**: feature | **Status**: unstarted

Implement password reset workflow with email verification, secure reset tokens, and token expiration. Include validation for new password strength requirements.

**Acceptance Criteria**:
1. Given a registered user email, When password reset is requested, Then a secure reset token is sent via email
2. Given a valid reset token and new password, When password reset is submitted, Then password is updated and user can login
3. Given an expired reset token, When password reset is attempted, Then a 401 Unauthorized error is returned
4. Given a weak new password, When password reset is submitted, Then a 400 Bad Request error is returned with strength requirements

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for password reset workflow`
- 🟢 GREEN: `green: password reset with email verification`
- 🔵 REFACTOR: `refactor: extract email service and token generation utilities`

**Branch**: `feature/1003-password-reset`

**Labels**: user-story, feature, authentication


## Epic #2000: Live Streaming Relay (Shoutcast Integration)

Implement Shoutcast source registration, relay worker orchestration, HLS segmentation with FFmpeg, and live stream delivery endpoints for both HLS and continuous MP3 formats.

### User Stories

#### Story #2001: As a DJ, I want to register my Shoutcast source so that my live stream can be relayed to listeners

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement stream registration endpoint that validates Shoutcast credentials, stores encrypted stream keys, and triggers Kubernetes relay worker pod creation with FFmpeg for HLS segmentation.

**Acceptance Criteria**:
1. Given valid Shoutcast URL and credentials, When DJ registers stream, Then stream record is created and relay worker pod is deployed
2. Given invalid Shoutcast credentials, When DJ attempts registration, Then a 422 Unprocessable Entity error is returned
3. Given a registered stream, When relay worker connects to Shoutcast, Then HLS segments are generated every 6 seconds
4. Given a registered stream, When relay worker is running, Then heartbeat is sent every 30 seconds with listener count

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for shoutcast stream registration`
- 🟢 GREEN: `green: stream registration with relay worker deployment`
- 🔵 REFACTOR: `refactor: extract kubernetes orchestration and improve validation`

**Branch**: `feature/2001-shoutcast-registration`

**Labels**: user-story, feature, live-streaming

#### Story #2002: As a listener, I want to access live HLS streams so that I can listen to DJ sets in real-time

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement HLS manifest serving endpoint with CDN caching, authentication for private streams, and subscription verification for monetized content. Relay workers continuously update manifests.

**Acceptance Criteria**:
1. Given a public live stream, When listener requests HLS manifest, Then manifest with TS segment URLs is returned
2. Given a private stream without authentication, When listener requests manifest, Then a 403 Forbidden error is returned
3. Given a monetized stream with valid subscription, When listener requests manifest, Then manifest is served from CDN cache
4. Given a relay worker generating segments, When manifest is requested, Then manifest lists last 3 segments with 30s TTL

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for HLS manifest serving`
- 🟢 GREEN: `green: HLS manifest endpoint with CDN caching`
- 🔵 REFACTOR: `refactor: improve authentication middleware and caching logic`

**Branch**: `feature/2002-hls-streaming`

**Labels**: user-story, feature, live-streaming

#### Story #2003: As a DJ, I want to view real-time stream metadata so that I can monitor my broadcast

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement stream status endpoint that queries Shoutcast admin API for current track and bitrate, and relay worker for listener count and connection status.

**Acceptance Criteria**:
1. Given an active stream, When DJ requests status, Then current track, bitrate, listener count, and uptime are returned
2. Given a stream with relay worker offline, When DJ requests status, Then a 503 Service Unavailable error is returned
3. Given a stream with Shoutcast source unreachable, When DJ requests status, Then relay_connected is false and error details are provided
4. Given stream metadata updates, When status is polled, Then data is refreshed from Shoutcast admin endpoint

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for stream status endpoint`
- 🟢 GREEN: `green: stream status with shoutcast metadata integration`
- 🔵 REFACTOR: `refactor: extract shoutcast client and improve error handling`

**Branch**: `feature/2003-stream-status`

**Labels**: user-story, feature, live-streaming


## Epic #3000: Audio File Upload & On-Demand Playlists

Implement direct MP3 file upload with virus scanning, background HLS transcoding via FFmpeg, playlist creation with mixed upload/external sources, and on-demand streaming endpoints.

### User Stories

#### Story #3001: As a DJ, I want to upload audio files so that I can create on-demand playlists

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement multipart file upload endpoint with validation, virus scanning, S3 storage, and background transcoding job to generate HLS segments. Support up to 100MB MP3 files.

**Acceptance Criteria**:
1. Given a valid MP3 file under 100MB, When DJ uploads file, Then file is stored in S3 and transcoding job is queued
2. Given an invalid file type, When DJ attempts upload, Then a 400 Bad Request error is returned
3. Given a file over 100MB, When DJ attempts upload, Then a 413 Payload Too Large error is returned
4. Given a queued transcoding job, When FFmpeg processes file, Then HLS segments are generated and upload status is set to ready

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for audio file upload`
- 🟢 GREEN: `green: multipart upload with S3 storage and transcoding queue`
- 🔵 REFACTOR: `refactor: extract file validation and improve error handling`

**Branch**: `feature/3001-audio-upload`

**Labels**: user-story, feature, file-upload

#### Story #3002: As a DJ, I want to create playlists with uploaded and external tracks so that I can offer on-demand content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement playlist creation endpoint that validates track sources (uploads or external URLs), stores playlist metadata, and pre-generates HLS manifests for ready tracks.

**Acceptance Criteria**:
1. Given valid uploaded tracks, When DJ creates playlist, Then playlist is created with HLS manifest pre-generated
2. Given external track URLs, When DJ creates playlist, Then URLs are validated via HEAD request before acceptance
3. Given a mix of upload and external tracks, When DJ creates playlist, Then playlist is created with both source types
4. Given invalid upload_id, When DJ creates playlist, Then a 422 Unprocessable Entity error is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist creation`
- 🟢 GREEN: `green: playlist creation with mixed track sources`
- 🔵 REFACTOR: `refactor: extract track validation and manifest generation`

**Branch**: `feature/3002-playlist-creation`

**Labels**: user-story, feature, playlists

#### Story #3003: As a listener, I want to stream on-demand playlists so that I can listen to DJ mixes anytime

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement HLS and continuous MP3 streaming endpoints for playlists with authentication for private content, subscription verification for monetized playlists, and CDN caching.

**Acceptance Criteria**:
1. Given a public playlist, When listener requests HLS manifest, Then pre-generated manifest with TS segments is served from CDN
2. Given a private playlist without authentication, When listener requests stream, Then a 403 Forbidden error is returned
3. Given a monetized playlist with valid subscription, When listener requests stream, Then continuous MP3 or HLS is served
4. Given a playlist with external tracks, When listener streams, Then tracks are proxied and concatenated seamlessly

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist streaming endpoints`
- 🟢 GREEN: `green: HLS and MP3 streaming with authentication`
- 🔵 REFACTOR: `refactor: improve caching strategy and proxy logic`

**Branch**: `feature/3003-playlist-streaming`

**Labels**: user-story, feature, playlists


## Epic #4000: Podcast Hosting & RSS with Custom Domains

Implement podcast series creation, episode management with upload/external sources, RSS 2.0 feed generation, custom domain registration with automated DNS/TLS provisioning via Let's Encrypt.

### User Stories

#### Story #4001: As a DJ, I want to create a podcast series with a custom domain so that I can host my podcast professionally

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement podcast creation endpoint with custom domain registration, DNS TXT record verification, CNAME creation, and automated TLS certificate provisioning via Let's Encrypt ACME.

**Acceptance Criteria**:
1. Given valid podcast metadata and custom domain, When DJ creates podcast, Then podcast record is created and DNS verification is initiated
2. Given a custom domain with valid TXT record, When DNS is verified, Then CNAME is created and TLS certificate is requested
3. Given a domain already in use, When DJ attempts registration, Then a 409 Conflict error is returned
4. Given successful TLS provisioning, When podcast is created, Then RSS feed URL is set to https://{custom_domain}/rss.xml

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for podcast creation with custom domain`
- 🟢 GREEN: `green: podcast creation with DNS and TLS automation`
- 🔵 REFACTOR: `refactor: extract DNS client and ACME integration`

**Branch**: `feature/4001-podcast-creation`

**Labels**: user-story, feature, podcasts

#### Story #4002: As a DJ, I want to add episodes to my podcast so that I can publish new content regularly

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement episode creation endpoint that validates upload or external media sources, stores episode metadata, and triggers RSS feed regeneration with proper iTunes tags.

**Acceptance Criteria**:
1. Given valid episode metadata with upload_id, When DJ adds episode, Then episode is created and RSS feed is regenerated
2. Given external media URL, When DJ adds episode, Then URL is validated via HEAD request before acceptance
3. Given monetization_required flag, When episode is added, Then RSS enclosure URL is set to signed URL for paywall
4. Given episode with explicit content, When RSS is generated, Then itunes:explicit tag is set correctly

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for episode creation`
- 🟢 GREEN: `green: episode creation with RSS regeneration`
- 🔵 REFACTOR: `refactor: extract RSS generator and improve validation`

**Branch**: `feature/4002-episode-creation`

**Labels**: user-story, feature, podcasts

#### Story #4003: As a podcast listener, I want to subscribe via RSS so that I can receive new episodes automatically

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement RSS 2.0 feed serving endpoint at custom domain with proper iTunes namespace, episode enclosures, and CDN caching. Support paywall for monetized episodes via signed URLs.

**Acceptance Criteria**:
1. Given a podcast with episodes, When listener requests RSS feed, Then valid RSS 2.0 XML with iTunes tags is returned
2. Given a custom domain, When RSS is requested at https://{domain}/rss.xml, Then feed is served with 300s CDN cache
3. Given monetized episodes, When listener without subscription requests RSS, Then enclosure URLs require authentication
4. Given RSS feed updates, When episode is added/updated, Then CDN cache is invalidated and feed is regenerated

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for RSS feed serving`
- 🟢 GREEN: `green: RSS 2.0 feed with iTunes tags and CDN caching`
- 🔵 REFACTOR: `refactor: improve RSS template and caching strategy`

**Branch**: `feature/4003-rss-feed`

**Labels**: user-story, feature, podcasts


## Epic #5000: Monetization & Ad Insertion

Implement Stripe integration for subscription plans, subscription management, paywall enforcement for streams/playlists/podcasts, and ad insertion hooks with configurable markers for live and on-demand content.

### User Stories

#### Story #5001: As a DJ, I want to create subscription plans so that I can monetize my content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement subscription plan creation endpoint with Stripe product/price creation, plan metadata storage, and billing interval configuration (monthly/yearly).

**Acceptance Criteria**:
1. Given valid plan details, When DJ creates plan, Then Stripe product and price are created and plan is stored in database
2. Given plan with features array, When plan is created, Then features are stored as JSONB in database
3. Given invalid price or currency, When DJ attempts plan creation, Then a 400 Bad Request error is returned
4. Given created plan, When retrieved, Then Stripe product_id and price_id are included in response

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for subscription plan creation`
- 🟢 GREEN: `green: subscription plan with Stripe integration`
- 🔵 REFACTOR: `refactor: extract Stripe client and improve error handling`

**Branch**: `feature/5001-subscription-plans`

**Labels**: user-story, feature, monetization

#### Story #5002: As a listener, I want to subscribe to a plan so that I can access premium content

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement subscription creation endpoint with Stripe customer creation, payment method attachment, subscription creation, and webhook handling for status updates.

**Acceptance Criteria**:
1. Given valid plan_id and payment method, When listener subscribes, Then Stripe subscription is created and status is active
2. Given payment failure, When listener attempts subscription, Then a 402 Payment Required error is returned
3. Given Stripe webhook for payment success, When webhook is received, Then subscription status is updated to active
4. Given Stripe webhook for subscription cancellation, When webhook is received, Then subscription status is updated to canceled

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for subscription creation and webhooks`
- 🟢 GREEN: `green: subscription with Stripe payment and webhook handling`
- 🔵 REFACTOR: `refactor: extract webhook handler and improve security`

**Branch**: `feature/5002-subscription-management`

**Labels**: user-story, feature, monetization

#### Story #5003: As a DJ, I want to configure ad insertion markers so that I can monetize streams with ads

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement ad marker configuration endpoint for live streams and playlists with position, duration, and ad type (pre-roll/mid-roll/post-roll). Relay workers insert ads at runtime.

**Acceptance Criteria**:
1. Given valid ad markers with positions, When DJ configures ads, Then markers are stored and relay worker is notified
2. Given ad marker at 300 seconds, When relay worker reaches position, Then FFmpeg switches input to ad source for specified duration
3. Given multiple ad markers, When stream plays, Then ads are inserted at all configured positions
4. Given ad insertion complete, When stream resumes, Then main feed continues seamlessly without gaps

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for ad insertion configuration`
- 🟢 GREEN: `green: ad marker configuration with relay worker integration`
- 🔵 REFACTOR: `refactor: improve ad scheduling logic and FFmpeg integration`

**Branch**: `feature/5003-ad-insertion`

**Labels**: user-story, feature, monetization


## Epic #6000: Analytics & Reporting

Implement analytics collection for live streams, playlists, and podcasts with date-range queries, aggregated metrics (listeners, plays, downloads), and exportable reports in JSON/CSV formats.

### User Stories

#### Story #6001: As a DJ, I want to view live stream analytics so that I can track listener engagement

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement analytics endpoint for live streams with date-range filtering, daily listener aggregation, peak listener calculation, and total listen count.

**Acceptance Criteria**:
1. Given a stream with analytics data, When DJ requests analytics with date range, Then daily listener counts are returned
2. Given analytics data, When DJ requests stream analytics, Then peak_listeners and total_listens are calculated
3. Given invalid date range, When DJ requests analytics, Then a 400 Bad Request error is returned
4. Given analytics data older than 2 years, When queried, Then anonymized aggregates are returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for stream analytics endpoint`
- 🟢 GREEN: `green: stream analytics with date-range filtering`
- 🔵 REFACTOR: `refactor: extract analytics aggregation utilities`

**Branch**: `feature/6001-stream-analytics`

**Labels**: user-story, feature, analytics

#### Story #6002: As a DJ, I want to view playlist play counts so that I can understand content popularity

**Points**: 3 | **Type**: feature | **Status**: unstarted

Implement analytics endpoint for playlists with date-range filtering, daily play count aggregation, and total plays calculation.

**Acceptance Criteria**:
1. Given a playlist with play data, When DJ requests analytics, Then daily play counts are returned
2. Given playlist analytics, When DJ requests with date range, Then only plays within range are included
3. Given multiple playlists, When DJ requests analytics, Then data is scoped to requested playlist only
4. Given analytics export request, When DJ requests CSV format, Then downloadable CSV file is generated

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist analytics endpoint`
- 🟢 GREEN: `green: playlist analytics with play count aggregation`
- 🔵 REFACTOR: `refactor: add CSV export and improve query performance`

**Branch**: `feature/6002-playlist-analytics`

**Labels**: user-story, feature, analytics

#### Story #6003: As a DJ, I want to view podcast download statistics so that I can measure episode success

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement analytics endpoint for podcasts with episode-level download counts, date-range filtering, and total downloads aggregation.

**Acceptance Criteria**:
1. Given a podcast with episodes, When DJ requests analytics, Then daily download counts per episode are returned
2. Given podcast analytics, When DJ requests with date range, Then downloads are filtered by publication date
3. Given multiple episodes, When DJ requests podcast analytics, Then total_downloads includes all episodes
4. Given analytics data, When DJ exports report, Then JSON and CSV formats are supported

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for podcast analytics endpoint`
- 🟢 GREEN: `green: podcast analytics with download tracking`
- 🔵 REFACTOR: `refactor: improve aggregation queries and add export formats`

**Branch**: `feature/6003-podcast-analytics`

**Labels**: user-story, feature, analytics

