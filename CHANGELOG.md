# Changelog

## v0.10.0

### Enhancements

- Use `uuid` data type for event correlation_id and causation_id ([#57](https://github.com/slashdotdash/eventstore/pull/57)).

  You will need to migrate your event store database using the following SQL statements:

  ```SQL
  ALTER TABLE events ALTER COLUMN correlation_id TYPE uuid USING correlation_id::uuid;
  ALTER TABLE events ALTER COLUMN causation_id TYPE uuid USING causation_id::uuid;
  ```

- Use [DBConnection](https://hexdocs.pm/db_connection/DBConnection.html)'s built in support for connection pools (using poolboy).

## v0.9.0

### Enhancements

- Adds `causation_id` alongside `correlation_id` for events ([#48](https://github.com/slashdotdash/eventstore/pull/48)).

  To migrate an existing event store database execute SQL statement:

  ```sql
  ALTER TABLE events ADD causation_id text;
  ```

- Allow single stream, and all streams, subscriptions to provide a mapper function that maps every received event before sending to the subscriber.

  ```elixir
  EventStore.subscribe_to_stream(stream_uuid, "subscription", subscriber, mapper: fn event -> event.data end)
  ```

- Subscribers now receive an `{:events, events}` tuple and should acknowledge receipt by: `EventStore.ack(subscription, events)`

## v0.8.1

### Enhancements

- Add Access functions to `EventStore.EventData` and `EventStore.RecordedEvent` modules ([#37](https://github.com/slashdotdash/eventstore/pull/37)).
- Allow database connection URL to be provided as a system variable ([#39](https://github.com/slashdotdash/eventstore/pull/39)).

### Bug fixes

- Writer not parsing database connection URL from config ([#38](https://github.com/slashdotdash/eventstore/pull/38/files)).

## v0.8.0

### Enhancements

- Stream events from a single stream forward.

## v0.7.4

### Enhancements

- Subscriptions use Elixir [streams](https://hexdocs.pm/elixir/Stream.html) to read events when catching up.

## v0.7.3

### Enhancements

- Upgrade `fsm` dependency to v0.3.0 to remove Elixir 1.4 compiler warnings.

## v0.7.2

### Enhancements

- Stream all events forward ([#34](https://github.com/slashdotdash/eventstore/issues/34)).

## v0.7.1

### Enhancements

- Allow snapshots to be deleted ([#26](https://github.com/slashdotdash/eventstore/issues/26)).

## v0.7.0

### Enhancements

- Subscribe to a single stream, or all streams, from a specified start position ([#17](https://github.com/slashdotdash/eventstore/issues/17)).

## v0.6.2

### Bug fixes

- Subscriptions that are at max capacity should wait until all pending events have been acknowledged by the subscriber being catching up with any unseen events.

## v0.6.1

### Enhancements

- Use IO lists to build insert events SQL statement ([#23](https://github.com/slashdotdash/eventstore/issues/23)).

## v0.6.0

### Enhancements

- Use `NaiveDateTime` for each recorded event's `created_at` property.

## v0.5.2

### Enhancements

- Provide typespecs for the public API ([#16](https://github.com/slashdotdash/eventstore/issues/16))
- Fix compilation warnings in mix database task ([#14](https://github.com/slashdotdash/eventstore/issues/14))

### Bug fixes

- Read stream forward does not use count to limit returned events ([#10](https://github.com/slashdotdash/eventstore/issues/10))

## v0.5.0

### Enhancements

- Ack handled events in subscribers ([#18](https://github.com/slashdotdash/eventstore/issues/18)).
- Buffer events between publisher and subscriber ([#19](https://github.com/slashdotdash/eventstore/issues/19)).
