# API Changelog

This page documents changes to the Zulip Server API over time. See also
the [Zulip server changelog][server-changelog].

The recommended way for a client like the Zulip mobile or desktop apps
that needs to support interaction with a wide range of different Zulip
server versions is to check the `zulip_feature_level` parameter in the
`/register` and `/server_settings` responses to determine which of the
below features are supported.

## Changes in Zulip 5.0

**Feature level 80**

* [`PATCH /settings`](/api/update-settings): The
  `/settings/notifications` and `/settings/display` endpoints were
  merged into the main `/settings` endpoint; now all personal settings
  should be edited using that single endpoint. The old URLs are
  preserved as deprecated aliases for backwards compatibility.

**Feature level 79**

* [`GET /users/me/subscriptions`](/api/get-subscriptions): The
  `subscribers` field now returns user IDs if `include_subscribers` is
  passed. Previously, this endpoint returned user display email
  addresses in this field.
* `GET /streams/{stream_id}/members`: This endpoint now returns user
  IDs. Previously, it returned display email addresses.

**Feature level 78**

* [`PATCH /settings`](/api/update-settings): Added
  `ignored_parameters_unsupported` field, which is a list of
  parameters that were ignored by the endpoint, to the response
  object.

* [`PATCH /settings`](/api/update-settings): Removed `full_name` and
  `account_email` fields from the response object.

**Feature level 77**

* [`GET /events`](/api/get-events): Removed `recipient_id` and
  `sender_id` field in responses of `delete_message` event when
  `message_type` is `private`.

**Feature level 76**

* [`POST /fetch_api_key`](/api/fetch-api-key), [`POST
  /dev_fetch_api_key`](/api/dev-fetch-api-key): The HTTP status for
  authentication errors is now 401. This was previously 403.
* All API endpoints now use the HTTP 401 error status for API requests
  involving a deactivated user or realm. This was previously 403.
* Mobile push notifications now include the `mentioned_user_group_id`
  and `mentioned_user_group_name` fields when a user group containing
  the user is mentioned.  Previously, these were indistinguishable
  from personal mentions (as both types have `trigger="mention"`).

**Feature level 75**

* [`POST /register`](/api/register-queue), `PATCH /realm`: Replaced `allow_community_topic_editing`
  field with an integer field `edit_topic_policy`.

**Feature level 74**

* [`POST /register`](/api/register-queue): Added `server_needs_upgrade`
  and `event_queue_longpoll_timeout_seconds` field when fetching
  realm data.

**Feature level 73**

* [`GET /users`](/api/get-users), [`GET /users/{user_id}`](/api/get-user),
  [`GET /users/{email}`](/api/get-user-by-email) and
  [`GET /users/me`](/api/get-own-user): Added `is_billing_admin` field to
  returned user objects.
* [`GET /events`](/api/get-events): Added `is_billing_admin` field to
  user objects sent in `realm_user` events.
* [`POST /register`](/api/register-queue): Added `is_billing_admin` field
  in the user objects returned in the `realm_users` field.

**Feature level 72**

* [`POST /register`](/api/register-queue): Renamed `max_icon_file_size` to
  `max_icon_file_size_mib`, `realm_upload_quota` to `realm_upload_quota_mib`
  and `max_logo_file_size` to `max_logo_file_size_mib`.

**Feature level 71**

* [`GET /events`](/api/get-events): Added `is_web_public` field to
  `stream` events changing `invite_only`.

**Feature level 70**

* [`POST /register`](/api/register-queue): Added new top-level
  `server_timestamp` field when fetching presence data, to match the
  existing presence API.

Feature levels 66-69 are reserved for future use in 4.x maintenance
releases.

## Changes in Zulip 4.0

**Feature level 65**

No changes; feature level used for Zulip 4.0 release.

**Feature level 64**

* `PATCH /streams/{stream_id}`: Removed unnecessary JSON-encoding of string
  parameters `new_name` and `description`.
* `PATCH /settings/display`: Removed unnecessary JSON-encoding of string
  parameters `default_view`, `emojiset` and `timezone`.

**Feature level 63**

* `PATCH /settings/notifications`: Removed unnecessary JSON-encoding of string
  parameter `notification_sound`.
* `PATCH /settings/display`: Removed unnecessary JSON-encoding of string
  parameter `default_language`.
* `POST /users/me/tutorial_status`: Removed unnecessary JSON-encoding of string
  parameter `status`.
* `POST /realm/domains`: Removed unnecessary JSON-encoding of string
  parameter `domain`.
* `PATCH /default_stream_groups/{user_id}`: Removed unnecessary JSON-encoding of string
  parameters `new_group_name` and `new_description`.
* `POST /users/me/hotspots`: Removed unnecessary JSON-encoding of string
  parameter `hotspot`.

**Feature level 62**

* Added `moderators only` option for `wildcard_mention_policy`.

**Feature level 61**

* Added support for inviting users as moderators to the invitation
  endpoints.

**Feature level 60**

* [`POST /register`](/api/register-queue): Added a new boolean field
  `is_moderator`, similar to the existing `is_admin`, `is_owner` and
  `is_guest` fields, to the response.
* [`PATCH /users/{user_id}`](/api/update-user): Added support for
  changing a user's organization-level role to moderator.
* API endpoints that return `role` values can now return `300`, the
  encoding of the moderator role.

**Feature level 59**

* [`GET /users`](/api/get-users), [`GET /users/{user_id}`](/api/get-user),
  [`GET /users/{email}`](/api/get-user-by-email) and
  [`GET /users/me`](/api/get-own-user): Added `role` field to returned
  user objects.
* [`GET /events`](/api/get-events): Added `role` field to
  user objects sent in `realm_user` events.
* [`POST /register`](/api/register-queue): Added `role` field
  in the user objects returned in the `realm_users` field.
* [`GET /events`](/api/get-events): Added new `zulip_version` and
  `zulip_feature_level` fields to the `restart` event.

**Feature level 58**

* [`POST /register`](/api/register-queue): Added the new
  `stream_typing_notifications` property to supported
  `client_capabilities`.
* [`GET /events`](/api/get-events): Extended format for `typing`
  events to support typing notifications in stream messages. These new
  events are only sent to clients with `client_capabilities`
  showing support for `stream_typing_notifications`.
* [`POST /set-typing-status`](/api/set-typing-status): Added support
  for sending typing notifications for stream messages.

**Feature level 57**

* [`PATCH /realm/filters/{filter_id}`](/api/update-linkifier): New
  endpoint added to update a realm linkifier.

**Feature level 56**

* [`POST /register`](/api/register-queue): Added a new setting
  `move_messages_between_streams_policy` for controlling who can
  move messages between streams.

**Feature level 55**

* [`POST /register`](/api/register-queue): Added `realm_giphy_rating`
  and `giphy_rating_options` fields.
* `PATCH /realm`: Added `giphy_rating` parameter.

**Feature level 54**

* `GET /realm/filters` has been removed and replace with [`GET
  /realm/linkifiers`](/api/get-linkifiers) which returns the data in a
  cleaner dictionary format.
* [`GET /events`](/api/get-events): Introduced new event type
  `realm_linkifiers`.  The previous `realm_filters` event type is
  still supported for backwards compatibility, but will be removed in
  a future release.
* [`POST /register`](/api/register-queue): The response now supports a
  `realm_linkifiers` event type, containing the same data as the
  legacy `realm_filters` key, with a more extensible object
  format. The previous `realm_filters` event type is still supported
  for backwards compatibility, but will be removed in a future
  release. The legacy `realm_filters` key is deprecated but remains
  available for backwards compatibility.

**Feature level 53**

* [`POST /register`](/api/register-queue): Added `max_topic_length`
  and `max_message_length`, and renamed `max_stream_name_length` and
  `max_stream_description_length` to allow clients to transparently
  support these values changing in a future server version.

**Feature level 52**

* `PATCH /realm`: Removed unnecessary JSON-encoding of string
  parameters `name`, `description`, `default_language`, and
  `default_code_block_language`.

**Feature level 51**

* [`POST /register`](/api/register-queue): Added a new boolean field
`can_invite_others_to_realm`.

**Feature level 50**

* [`POST /register`](/api/register-queue): Replaced `invite_by_admins_only`
field with an integer field `invite_to_realm_policy`.

**Feature level 49**

* Added new [`POST /realm/playground`](/api/add-code-playground) and
  [`DELETE /realm/playground/{playground_id}`](/api/remove-code-playground)
  endpoints for code playgrounds.
* [`GET /events`](/api/get-events): A new `realm_playgrounds` events
  is sent when changes are made to a set of configured code playgrounds for
  an organization.
* [`POST /register`](/api/register-queue): Added a new `realm_playgrounds`
  field, which is required to fetch the set of configured code playgrounds for
  an organization.

**Feature level 48**

* [`POST /users/me/muted_users/{muted_user_id}`](/api/mute-user),
  [`DELETE /users/me/muted_users/{muted_user_id}`](/api/unmute-user):
  New endpoints added to mute/unmute users.
* [`GET /events`](/api/get-events): Added new event type `muted_users`
  which will be sent to a user when the set of users muted by them has
  changed.

**Feature level 47**

* [`POST /register`](/api/register-queue): Added a new `giphy_api_key`
  field, which is required to fetch GIFs using the GIPHY API.

**Feature level 46**

* [`GET /messages`](/api/get-messages) and [`GET
  /events`](/api/get-events): The `topic_links` field now contains a
  list of dictionaries, rather than a list of strings.

**Feature level 45**

* [`GET /events`](/api/get-events): Removed useless `op` field from
  `custom_profile_fields` events.  These events contain the full set
  of configured `custom_profile_fields` for the organization
  regardless of what triggered the change.

**Feature level 44**

* [`POST /register`](/api/register-queue): extended the `unread_msgs`
  object to include `old_unreads_missing`, which indicates whether the
  server truncated the `unread_msgs` due to excessive total unread
  messages.

**Feature level 43**

* [`GET /users/{user_id_or_email}/presence`]: Added support for
 passing the `user_id` to identify the target user.

**Feature level 42**

* `PATCH /settings/display`: Added a new `default_view` setting allowing
  the user to [set the default view](/help/change-default-view).

**Feature level 41**

* [`GET /events`](/api/get-events): Removed `name` field from update
  subscription events.

**Feature level 40**

* [`GET /events`](/api/get-events): Removed `email` field from update
  subscription events.

**Feature level 39**

* Added new [GET /users/{email}](/api/get-user-by-email) endpoint.

**Feature level 38**

* [`POST /register`](/api/register-queue): Increased
  `realm_community_topic_editing_limit_seconds` time limit value
  to 259200s (3 days).

**Feature level 37**

* Consistently provide `subscribers` in stream data when
  clients register for subscriptions with `include_subscribers`,
  even if the user can't access subscribers.

**Feature level 36**

* [`POST /users`](/api/create-user): Restricted access to organization
  administrators with the `can_create_users` permission.
* [Error handling](/api/rest-error-handling): The `code` property will
  not be present in errors due to rate limits.

**Feature level 35**

* The peer_add and peer_remove subscription events now have plural
  versions of `user_ids` and `stream_ids`.

**Feature level 34**

* [`POST /register`](/api/register-queue): Added a new `wildcard_mention_policy`
  setting for controlling who can use wildcard mentions in large streams.

**Feature level 33**

* Markdown code blocks now have a `data-code-language` attribute
  attached to the outer `div` element, recording the programming
  language that was selecting for syntax highlighting.  This field
  supports the upcoming "view in playground" feature for code blocks.

**Feature level 32**

* [`GET /events`](/api/get-events): Added `op` field to
  `update_message_flags` events, deprecating the `operation` field
  (which has the same value).  This removes an unintentional anomaly
  in the format of this event type.

**Feature level 31**

* [`GET users/me/subscriptions`](/api/get-subscriptions): Added a
  `role` field to Subscription objects representing whether the user
  is a stream administrator.

* [`GET /events`](/api/get-events): Added `role` field to
  Subscription objects sent in `subscriptions` events.

Note that as of this feature level, stream administrators are a
partially completed feature.  In particular, it is impossible for a
user to be a stream administrator at this feature level.

**Feature level 30**

* [`GET users/me/subscriptions`](/api/get-subscriptions), [`GET
  /streams`](/api/get-streams): Added `date_created` to Stream
  objects.
* [`POST /users`](/api/create-user), `POST /bots`: The ID of the newly
  created user is now returned in the response.

Feature levels 28 and 29 are reserved for future use in 3.x bug fix
releases.

## Changes in Zulip 3.1

**Feature level 27**

* The `short_name` field is removed from `display_recipients`
  in `POST /users`.

**Feature level 26**

* The `sender_short_name` field is no longer included in
  `GET /messages`.
* The `short_name` field is removed from `display_recipients`
  in `GET /messages`.

## Changes in Zulip 3.0

**Feature level 25**

No changes; feature level used for Zulip 3.0 release.

**Feature level 24**

* The `!avatar()` and `!gravatar()` Markdown syntax, which was never
  documented, had inconsistent syntax, and was rarely used, was
  removed.

**Feature level 23**

* `GET/PUT/POST /users/me/pointer`: Removed.  Zulip 3.0 removes the
  `pointer` concept from Zulip; this legacy data structure was
  replaced by tracking unread messages and loading views centered on
  the first unread message.

**Feature level 22**

* [`GET /attachments`](/api/get-attachments): The date when a message
  using the attachment was sent is now correctly encoded as
  `date_sent`, replacing the confusingly named `name` field.  The
  `date_sent` and `create_time` fields of attachment objects are now
  encoded as integers; (previously the implementation could send
  floats incorrectly suggesting that microsecond precision is
  relevant).
* `GET /invites`: Now encodes the user ID of the person who created
   the invitation as `invited_by_user_id`, replacing the previous
   `ref` field (which had that user's Zulip display email address).

**Feature level 21**

* `PATCH /settings/display`: Replaced the `night_mode` boolean with
  `color_scheme` as part of supporting automatic night theme detection.

**Feature level 20**

* Added support for inviting users as organization owners to the
  invitation endpoints.

**Feature level 19**

* [`GET /events`](/api/get-events): `subscriptions` event with
  `op="peer_add"` and `op="peer_remove"` now identify the modified
  stream by a `stream_id` field, replacing the old `name` field.

**Feature level 18**

* [`POST /register`](/api/register-queue): Added
  `user_avatar_url_field_optional` to supported `client_capabilities`.

**Feature level 17**

* [`GET users/me/subscriptions`](/api/get-subscriptions),
  [`GET /streams`](/api/get-streams): Added
  `message_retention_days` to Stream objects.
* [`POST users/me/subscriptions`](/api/subscribe), [`PATCH
  streams/{stream_id}`](/api/update-stream): Added `message_retention_days`
  parameter.

**Feature level 16**

* [`GET /users/me`]: Removed `pointer` from the response, as the
  "pointer" concept is being removed in Zulip.
* Changed the rendered HTML markup for mentioning a time to use the
  `<time>` HTML tag.  It is OK for clients to ignore the previous time
  mention markup, as the feature was not advertised before this change.

**Feature level 15**

* Added [spoilers](/help/format-your-message-using-markdown#spoilers)
  to supported Markdown features.

**Feature level 14**

* [`GET users/me/subscriptions`](/api/get-subscriptions): Removed
  the `is_old_stream` field from Stream objects.  This field was
  always equivalent to `stream_weekly_traffic != null` on the same object.

**Feature level 13**

* [`POST /register`](/api/register-queue): Added
  `bulk_message_deletion` to supported `client_capabilities`.
* [`GET /events`](/api/get-events): `message_deleted`
  events have new behavior.  The `sender` and `sender_id` fields were
  removed, and the `message_id` field was replaced by a `message_ids`
  list for clients with the `bulk_message_deletion` client capability.
  All clients should upgrade; we expect `bulk_message_deletion` to be
  required in the future.

**Feature level 12**

* [`GET users/{user_id}/subscriptions/{stream_id}`](/api/get-subscription-status):
  New endpoint added for checking if another user is subscribed to a stream.

**Feature level 11**

* [`POST /register`](/api/register-queue): Added
  `realm_community_topic_editing_limit_seconds` to the response, the
  time limit before community topic editing is forbidden.  A `null`
  value means no limit. This was previously hard-coded in the server
  as 86400 seconds (1 day).
* [`POST /register`](/api/register-queue): The response now contains a
  `is_owner`, similar to the existing `is_admin` and `is_guest` fields.
* [`POST /set-typing-status`](/api/set-typing-status): Removed legacy support for sending email
  addresses, rather than user IDs, to encode private message recipients.

**Feature level 10**

* [`GET users/me`](/api/get-own-user): Added `avatar_version`, `is_guest`,
  `is_active`, `timezone`, and `date_joined` fields to the User objects.
* [`GET users/me`](/api/get-own-user): Removed `client_id` and `short_name`
  from the response to this endpoint.  These fields had no purpose and
  were inconsistent with other API responses describing users.

**Feature level 9**

* [`POST users/me/subscriptions`](/api/subscribe), [`DELETE
  /users/me/subscriptions`](/api/unsubscribe): Other users to
  subscribe/unsubscribe, declared in the `principals` parameter, can
  now be referenced by user_id, rather than Zulip display email
  address.
* [PATCH /messages/{message_id}](/api/update-message): Added
  `send_notification_to_old_thread` and
  `send_notification_to_new_thread` optional parameters.

**Feature level 8**

* [`GET /users`](/api/get-users), [`GET /users/{user_id}`](/api/get-user)
  and [`GET /users/me`](/api/get-own-user): User objects now contain the
  `is_owner` field as well.
* Added [time mentions](/help/format-your-message-using-markdown#mention-a-time)
  to supported Markdown features.

**Feature level 7**

* [`GET /events`](/api/get-events): `realm_user` and
  `realm_bot` events no longer contain an `email` field to identify
  the user; use the `user_id` field instead.  Previously, some (but
  not all) events of these types contained an `email` key in addition to
  to `user_id`) for identifying the modified user.
* [`PATCH /users/{user_id}`](/api/update-user): The `is_admin` and
  `is_guest` parameters were removed in favor of the more general
  `role` parameter for specifying a change in user role.
* [`GET /events`](/api/get-events): `realm_user` events
  sent when a user's role changes now include `role` property, instead
  of the previous `is_guest` or `is_admin` booleans.
* `GET /realm/emoji`: The user who uploaded a given custom emoji is
  now indicated by an `author_id` field, replacing a previous `author`
  object with unnecessary additional data.

**Feature level 6**

* [`GET /events`](/api/get-events): `realm_user` events to
  update a user's avatar now include the `avatar_version` field, which
  is important for correctly refetching medium-size avatar images when
  the user's avatar changes.
* [`GET /users`](/api/get-users) and [`GET
  /users/{user_id}`](/api/get-user): User objects now contain the
  `avatar_version` field as well.

**Feature level 5**
* [`GET /events`](/api/get-events): `realm_bot` events,
  sent when changes are made to bot users, now contain an
  integer-format `owner_id` field, replacing the `owner` field (which
  was an email address).

**Feature level 4**

* `jitsi_server_url`, `development_environment`, `server_generation`,
  `password_min_length`, `password_min_guesses`, `max_file_upload_size_mib`,
  `max_avatar_file_size_mib`, `server_inline_image_preview`,
  `server_inline_url_embed_preview`, `server_avatar_changes_disabled` and
  `server_name_changes_disabled` fields are now available via
  `POST /register` to make them accessible to all the clients;
  they were only internally available to Zulip's web app prior to this.

**Feature level 3**:

* `zulip_version` and `zulip_feature_level` are always returned
  in `POST /register`; previously they were only returned if `event_types`
  included `zulip_version`.
* Added new `presence_enabled` user notification setting; previously
  [presence](/help/status-and-availability) was always enabled.

**Feature level 2**:

* [`POST /messages/{message_id}/reactions`](/api/add-reaction):
  The `reaction_type` parameter is optional; the server will guess the
  `reaction_type` if it is not specified (checking custom emoji, then
  Unicode emoji for any with the provided name).
* `reactions` objects returned by the API (both in `GET /messages` and
  in `GET /events`) now include the user who reacted in a top-level
  `user_id` field.  The legacy `user` dictionary (which had
  inconsistent format between those two endpoints) is deprecated.

**Feature level 1**:

* [`GET /server_settings`](/api/get-server-settings): Added
  `zulip_feature_level`, which can be used by clients to detect which
  of the features described in this changelog are supported.
* [`POST /register`](/api/register-queue): Added `zulip_feature_level`
  to the response if `zulip_version` is among the requested
  `event_types`.
* [`GET /users`](/api/get-users): User objects for bots now
  contain a `bot_owner_id`, replacing the previous `bot_owner` field
  (which had the email address of the bot owner).
* [`GET /users/{user_id}`](/api/get-user): Endpoint added.
* [`GET /messages`](/api/get-messages): Add support for string-format
  values for the `anchor` parameter, deprecating and replacing the
  `use_first_unread_anchor` parameter.
* [`GET /messages`](/api/get-messages) and [`GET
  /events`](/api/get-events): Message objects now use
  `topic_links` rather than `subject_links` to indicate links either
  present in the topic or generated by linkifiers applied to the topic.
* [`POST /users/me/subscriptions`](/api/subscribe): Replaced
  `is_announcement_only` boolean with `stream_post_policy` enum for
  specifying who can post to a stream.
* [`PATCH /streams/{stream_id}`](/api/update-stream): Replaced
  `is_announcement_only` boolean with `stream_post_policy` enum for
  specifying who can post to a stream.
* [`GET /streams`](/api/get-streams): Replaced
  `is_announcement_only` boolean with `stream_post_policy` enum for
  specifying who can post to a stream.
* `GET /api/v1/user_uploads`: Added new endpoint for requesting a
  temporary URL for an uploaded file that does not require
  authentication to access (e.g. for passing from a Zulip desktop,
  mobile, or terminal app to the user's default browser).
* Added `EMAIL_ADDRESS_VISIBILITY_NOBODY` possible value for
  `email_address_visibility`.
* Added `private_message_policy` realm setting.
* `muted_topic` objects now are a 3-item tuple: (`stream_id`, `topic`,
  `date_muted`).  Previously, they were a 2-item tuple.
* `GitLab` authentication is now available.
* Added `None` as a video call provider option.

## Changes in Zulip 2.1

* [`GET /users`](/api/get-users): Added `include_custom_profile_fields`
  to request custom profile field data.
* [`GET /users/me`](/api/get-own-user): Added `avatar_url` field,
  containing the user's avatar URL, to the response.
* [`GET /users/me/subscriptions`](/api/get-subscriptions): Added
  `include_subscribers` parameter controlling whether data on the
  other subscribers is included.  Previous behavior was to always send
  subscriber data.
* [`GET /users/me/subscriptions`](/api/get-subscriptions):
  Stream-level notification settings like `push_notifications` were
  changed to be nullable boolean fields (true/false/null), with `null`
  meaning that the stream inherits the organization-level default.
  Previously, the only values were true/false.  A client communicates
  support for this feature using `client_capabilities`.
* [`GET /users/me/subscriptions`](/api/get-subscriptions): Added
  `wildcard_mentions_notify` notification setting, with the same
  global-plus-stream-level-override model as other notification settings.
* [`GET /server_settings`](/api/get-server-settings): Added
  `external_authentication_methods` structure, used to display login
  buttons nicely in the mobile apps.
* Added `first_message_id` field to Stream objects.  This is helpful
  for determining whether the stream has any messages older than a
  window cached in a client.
* Added `is_web_public` field to Stream objects.  This field is
  intended to support web-public streams.
* Added `/export/realm` endpoints for triggering a data export.
* `PATCH /realm`: Added `invite_to_stream_policy`,
  `create_stream_policy`, `digest_emails_enabled`, `digest_weekday`,
  `user_group_edit_policy`, and `avatar_changes_disabled` organization settings.
* Added `fluid_layout_width`, `desktop_icon_count_display`, and
  `demote_inactive_streams` display settings.
* `enable_stream_sounds` was renamed to
  `enable_stream_audible_notifications`.
* Deprecated `in_home_view`, replacing it with the more readable
  `is_muted` (with the opposite meaning).
* Custom profile fields: Added `EXTERNAL_ACCOUNT` field type.

## Changes in Zulip 2.0

* [`POST /messages`](/api/send-message): Added support for using user
  IDs and stream IDs for specifying the recipients of a message.
* [`POST /messages`](/api/send-message), [`POST
  /messages/{message_id}`](/api/update-message): Added support for
  encoding topics using the `topic` parameter name.  The previous
  `subject` parameter name was deprecated but is still supported for
  backwards-compatibility.
* [`POST /set-typing-status`](/api/set-typing-status): Added support for specifying the
  recipients with user IDs, deprecating the original API of specifying
  them using email addresses.

------------------

## Changes not yet stabilized

* [`POST /register`](/api/register-queue): Added `slim_presence`
  parameter.  Changes the format of presence events, but is still
  being changed and should not be used by clients.

[server-changelog]: https://zulip.readthedocs.io/en/latest/overview/changelog.html
