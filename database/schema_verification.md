# Database Schema Verification Report

## Connection Details
- **Host:** localhost
- **Port:** 5000
- **Database:** myapp
- **User:** appuser
- **Connection String:** `psql postgresql://appuser:dbuser123@localhost:5000/myapp`

## Verified Tables (6 total)

### 1. players
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| name | varchar(100) | NOT NULL | |
| player_type | varchar(20) | NOT NULL | 'human' |
| ai_difficulty | varchar(20) | | |
| created_at | timestamptz | | now() |

**Seed Data:** 4 rows (2 human players, 2 AI players with easy/hard difficulty)

### 2. games
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| mode | varchar(50) | NOT NULL | 'classic' |
| status | varchar(20) | NOT NULL | 'waiting' |
| current_turn_player_id | uuid | | |
| winner_id | uuid | | |
| created_at | timestamptz | | now() |
| updated_at | timestamptz | | now() |
| ended_at | timestamptz | | |

**Indexes:** games_pkey, idx_games_status
**Seed Data:** 2 rows (1 in_progress, 1 completed)

### 3. game_players
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| game_id | uuid | NOT NULL | |
| player_id | uuid | NOT NULL | |
| score | integer | NOT NULL | 0 |
| turn_order | integer | NOT NULL | 0 |
| created_at | timestamptz | | now() |

**Constraints:** UNIQUE(game_id, player_id), FK to games and players
**Seed Data:** 4 rows

### 4. game_settings
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| game_id | uuid | NOT NULL | |
| sound_enabled | boolean | | true |
| physics_speed | double precision | | 1.0 |
| board_friction | double precision | | 0.98 |
| striker_max_force | double precision | | 100.0 |
| timer_per_turn | integer | | 30 |
| scoring_mode | varchar(50) | | 'standard' |
| created_at | timestamptz | | now() |
| updated_at | timestamptz | | now() |

**Constraints:** UNIQUE(game_id), FK to games
**Seed Data:** 2 rows

### 5. turns
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| game_id | uuid | NOT NULL | |
| player_id | uuid | NOT NULL | |
| turn_number | integer | NOT NULL | |
| striker_position_x | double precision | | |
| striker_position_y | double precision | | |
| striker_angle | double precision | | |
| striker_force | double precision | | |
| coins_pocketed | text[] | | '{}' |
| foul | boolean | | false |
| foul_reason | varchar(200) | | |
| points_earned | integer | | 0 |
| created_at | timestamptz | | now() |

**Indexes:** turns_pkey, idx_turns_game_id
**Seed Data:** 3 rows

### 6. analytics_events
| Column | Type | Nullable | Default |
|--------|------|----------|---------|
| id | uuid | NOT NULL | uuid_generate_v4() |
| game_id | uuid | | |
| player_id | uuid | | |
| event_type | varchar(100) | NOT NULL | |
| event_data | jsonb | | '{}' |
| session_id | varchar(100) | | |
| device_info | jsonb | | '{}' |
| created_at | timestamptz | | now() |

**Indexes:** analytics_events_pkey, idx_analytics_events_created_at, idx_analytics_events_game_id, idx_analytics_events_type
**Seed Data:** 4 rows

## Extensions
- `uuid-ossp` (for uuid_generate_v4)

## Verification Status: ✅ PASSED
All 6 tables created with correct schema, indexes, foreign keys, and seed data confirmed present.
