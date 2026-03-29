```sql
create type user_status as enum('Онлайн','Офлайн');

create table if not exists "user"(
	user_id SERIAL PRIMARY KEY,
	user_nickname varchar(20) UNIQUE NOT NULL,
	user_email varchar(40) UNIQUE NOT NULL,
	user_password varchar(256) NOT NULL,
	status user_status NOT NULL,
	user_starting_date date NOT NULL
);

create table if not exists guild(
	guild_id SERIAL PRIMARY KEY,
	guild_name varchar(50) NOT NULL UNIQUE,
	guild_description text,
	guild_lvl int NOT NULL check(guild_lvl >= 1 and guild_lvl <= 50),
	guild_date_start date NOT NULL,
	guild_reputation int NOT NULL check(guild_reputation >= 0 and guild_reputation <= 10000)
);

create type player_class as enum('Маг','Воін','Стрелок');

create table if not exists player(
	player_id SERIAL PRIMARY KEY,
	player_name varchar(30) NOT NULL UNIQUE,
	player_characteristics text NOT NULL,
	pclass player_class NOT NULL,
	player_lvl int NOT NULL check(player_lvl >= 1 and player_lvl <= 30),
	user_id int NOT NULL references "user"(user_id),
	guild_id int NULL references guild(guild_id)
);

create table if not exists item(
	item_id SERIAL PRIMARY KEY,
	item_name varchar(30) NOT NULL,
	item_description text,
	item_characteristics text NOT NULL,
	item_cost int,
	Item_tradeable BOOLEAN NOT NULL	
);

create table if not exists player_item(
	player_id int references player(player_id),
	item_id int references item(item_id),
	quantity int NOT NULL DEFAULT 1,
	PRIMARY KEY(player_id, item_id)
);

create type local_size as enum('Мала','Середня','Велика');
create type local_complexity as enum('Легка','Середня','Важка');

create table if not exists "location"(
	location_id SERIAL PRIMARY KEY,
	location_name varchar(20) NOT NULL UNIQUE,
	location_size local_size NOT NULL,
	location_complexity local_complexity NOT NULL,
	location_min_lvl int NOT NULL check(location_min_lvl >= 1)
);

create table if not exists location_guild(
	guild_id int UNIQUE references guild(guild_id),
	location_id int UNIQUE references "location"(location_id)
);

create type creature_types as enum('Тварина','Елементаль','Демон','Механізм','Мурлок');
create type creature_types_damage as enum('Вогняний','Елементальний','Ближній','Дальній','Магічний');

create table if not exists creature(
	creature_id SERIAL PRIMARY KEY,
	creature_name varchar(30) NOT NULL UNIQUE,
	creature_description text,
	creature_type creature_types NOT NULL,
	creature_characteristics text NOT NULL,
	creature_type_damage creature_types_damage NOT NULL
);

create table if not exists creature_location(
	creature_id int references creature(creature_id),
	location_id int references "location"(location_id),
	spawn_time int,
	PRIMARY KEY (creature_id, location_id)
);

create table if not exists creature_item(
	creature_id int references creature(creature_id),
	item_id int references item(item_id),
	PRIMARY KEY (creature_id, item_id)
);

create table if not exists quest(
	quest_id SERIAL PRIMARY KEY,
	quest_name varchar(40) NOT NULL UNIQUE,
	quest_description text NOT NULL,
	quest_complexity local_complexity NOT NULL,
	quest_repeatable BOOLEAN NOT NULL
);

create table if not exists award(
	quest_id int references quest(quest_id),
	item_id int references item(item_id) UNIQUE,
	PRIMARY KEY (quest_id, item_id)
);

create table if not exists quest_location(
	quest_id int references quest(quest_id),
	location_id int references "location"(location_id),
	PRIMARY KEY (quest_id, location_id)
);

create table if not exists killing(
	quest_id int references quest(quest_id),
	creature_id int references creature(creature_id),
	PRIMARY KEY (quest_id, creature_id)
);

create table if not exists quest_guild(
	quest_id int references quest(quest_id),
	guild_id int references guild(guild_id),
	PRIMARY KEY (quest_id, guild_id)
);
```
