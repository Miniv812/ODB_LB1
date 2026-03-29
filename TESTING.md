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
```
