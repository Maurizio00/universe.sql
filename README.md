# universe.sql
camper: /project$ psql --username=freecodecamp --dbname=postgres
Border style is 2.
Pager usage is off.
psql (12.17 (Ubuntu 12.17-1.pgdg22.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=> CREATE DATABASE universe;
CREATE DATABASE
postgres=> \c universe
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
You are now connected to database "universe" as user "freecodecamp".
universe=> CREATE TABLE galaxy (  galaxy_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  galaxy_type TEXT NOT NULL,  age_in_millions_of_years NUMERIC NOT NULL,  distance_from_earth INT NOT NULL,  has_life BOOLEAN DEFAULT FALSE);
CREATE TABLE
universe=> INSERT INTO galaxy (name, galaxy_type, age_in_millions_of_years, distance_from_earth, has_life) VALUES('Milky Way', 'Spiral', 13600, 0, TRUE),('Andromeda', 'Spiral', 10000, 2537000, FALSE),('Triangulum', 'Spiral', 12000, 3000000, FALSE),('Whirlpool', 'Spiral', 400, 23000000, FALSE),('Sombrero', 'Elliptical', 8900, 29000000, FALSE),('Messier 87', 'Elliptical', 13000, 53000000, FALSE);
INSERT 0 6
universe=> CREATE TABLE star (  star_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  galaxy_id INT REFERENCES galaxy(galaxy_id) ON DELETE CASCADE,  star_type TEXT NOT NULL,  age_in_millions_of_years NUMERIC NOT NULL,  is_spherical BOOLEAN DEFAULT TRUE);
CREATE TABLE
universe=> INSERT INTO star (name, galaxy_id, star_type, age_in_millions_of_years, is_spherical) VALUES('Sun', 1, 'G-type Main Sequence', 4600, TRUE),('Proxima Centauri', 1, 'Red Dwarf', 4850, TRUE),('Sirius', 1, 'White Dwarf', 242, TRUE),('Vega', 1, 'Main Sequence', 455, TRUE),('Betelgeuse', 1, 'Red Supergiant', 8, TRUE),('Rigel', 1, 'Blue Supergiant', 8, TRUE);
INSERT 0 6
universe=> CREATE TABLE planet (  planet_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  star_id INT REFERENCES star(star_id) ON DELETE CASCADE,  planet_type TEXT NOT NULL,  has_life BOOLEAN DEFAULT FALSE,  distance_from_star INT NOT NULL);
CREATE TABLE
universe=> INSERT INTO planet (name, star_id, planet_type, has_life, distance_from_star) VALUES('Earth', 1, 'Terrestrial', TRUE, 1),('Mars', 1, 'Terrestrial', FALSE, 2),('Venus', 1, 'Terrestrial', FALSE, 1),('Jupiter', 1, 'Gas Giant', FALSE, 5),('Saturn', 1, 'Gas Giant', FALSE, 9),('Neptune', 1, 'Ice Giant', FALSE, 30),('Kepler-22b', 2, 'Super-Earth', UNKNOWN, 500),('Proxima b', 2, 'Super-Earth', FALSE, 0),('Sirius b', 3, 'Gas Giant', FALSE, 3),('Vega I', 4, 'Terrestrial', FALSE, 10),('Betelgeuse c', 5, 'Gas Giant', FALSE, 15),('Rigel d', 6, 'Super-Earth', FALSE, 20);
ERROR:  column "unknown" does not exist
LINE 1: ...ant', FALSE, 30),('Kepler-22b', 2, 'Super-Earth', UNKNOWN, 5...
                                                             ^
universe=> INSERT INTO planet (name, star_id, planet_type, has_life, distance_from_star) VALUES('Earth', 1, 'Terrestrial', TRUE, 1),('Mars', 1, 'Terrestrial', FALSE, 2),('Venus', 1, 'Terrestrial', FALSE, 1),('Jupiter', 1, 'Gas Giant', FALSE, 5),('Saturn', 1, 'Gas Giant', FALSE, 9),('Neptune', 1, 'Ice Giant', FALSE, 30),('Kepler-22b', 2, 'Super-Earth', FALSE , 500)
,('Proxima b', 2, 'Super-Earth', FALSE, 0),('Sirius b', 3, 'Gas Giant', FALSE, 3),('Vega I', 4, 'Terrestrial', FALSE, 10),
('Betelgeuse c', 5, 'Gas Giant', FALSE, 15),('Rigel d', 6, 'Super-Earth', FALSE, 20);
INSERT 0 12
universe=> CREATE TABLE moon (  moon_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  planet_id INT REFERENCES planet(planet_id) ON DELETE CASCADE,  diameter_km INT NOT NULL,  is_spherical BOOLEAN DEFAULT TRUE);
CREATE TABLE
universe=> INSERT INTO moon (name, planet_id, diameter_km, is_spherical) VALUES('Moon', 1, 3474, TRUE),('Phobos', 2, 22, FALSE),('Deimos', 2, 12, FALSE),('Io', 4, 3643, TRUE),('Europa', 4, 3121, TRUE),('Ganymede', 4, 5268, TRUE),('Callisto', 4, 4821, TRUE),('Titan', 5, 5150, TRUE),('Enceladus', 5, 504, TRUE),('Triton', 6, 2706, TRUE),('Nereid', 6, 340, FALSE),('Kepler-22b I', 7, 1500, TRUE),('Proxima b I', 8, 500, FALSE),('Sirius b I', 9, 800, TRUE),('Vega I-a', 10, 1000, TRUE),('Betelgeuse c I', 11, 1200, TRUE),('Rigel d I', 12, 1300, TRUE),('Rigel d II', 12, 800, TRUE),('Rigel d III', 12, 600, FALSE),('Rigel d IV', 12, 400, FALSE);
INSERT 0 20
universe=> CREATE TABLE space_mission (  mission_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  launch_year INT NOT NULL,  target_type TEXT NOT NULL CHECK (target_type IN ('galaxy', 'star', 'planet', 'moon')),  target_id INT NOT NULL,  success BOOLEAN DEFAULT TRUE,  description TEXT);
CREATE TABLE
universe=> INSERT INTO space_mission (name, launch_year, target_type, target_id, success, description) VALUES('Apollo 11', 1969, 'moon', 1, TRUE, 'Primera misión tripulada en llegar a la Luna.'),('Voyager 1', 1977, 'planet', 4, TRUE, 'Exploró Júpiter y Saturno antes de salir del sistema solar.'),('Voyager 2', 1977, 'planet', 6, TRUE, 'Visitó Urano y Neptuno, sigue en el espacio interestelar.'),('Hubble Telescope', 1990, 'galaxy', 1, TRUE, 'Observa galaxias lejanas desde la órbita terrestre.'),('Kepler Space Telescope', 2009, 'star', 2, TRUE, 'Descubrió miles de exoplanetas en otras estrellas.'),('Perseverance Rover', 2020, 'planet', 2, TRUE, 'Explora la superficie de Marte buscando signos de vida.'),('New Horizons', 2006, 'planet', 6, TRUE, 'Primera misión en sobrevolar Plutón y el cinturón de Kuiper.'),('James Webb Telescope', 2021, 'galaxy', 1, TRUE, 'Observa las primeras galaxias formadas en el universo.'),('Luna 2', 1959, 'moon', 1, TRUE, 'Primera nave en impactar la superficie de la Luna.'),('Galileo Probe', 1989, 'planet', 4, TRUE, 'Orbitó Júpiter y exploró sus lunas.');
INSERT 0 10
universe=> ALTER TABLE space_mission DELETE COLUMN target_id;
ERROR:  syntax error at or near "DELETE"
LINE 1: ALTER TABLE space_mission DELETE COLUMN target_id;
                                  ^
universe=> ALTER TABLE space_mission DROP COLUMN target_id;
ALTER TABLE
universe=> ALTER TABLE space_mission ADD COLUMN  target_fk INT NOT NULL;
ERROR:  column "target_fk" contains null values
universe=> ALTER TABLE space_mission ADD COLUMN  target_fk INT;
ALTER TABLE
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES galaxy(galaxy_id) ON DELETE CASCADE,  FOREIGN KEY (target_fk) REFERENCES star(star_id) ON DELETE CASCADE,  FOREIGN KEY (target_fk) REFERENCES planet(planet_id) ON DELETE CASCADE,  FOREIGN KEY (target_fk) REFERENCES moon(moon_id) ON DELETE CASCADE);
ERROR:  syntax error at or near "FOREIGN"
LINE 1: ... REFERENCES galaxy(galaxy_id) ON DELETE CASCADE,  FOREIGN KE...
                                                             ^
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES galaxy(galaxy_id) ON DELETE CASCADE;
ALTER TABLE
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES star(star_id) ON DELETE CASCADE
universe-> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES star(star_id) ON DELETE CASCADE;
ERROR:  syntax error at or near "ALTER"
LINE 2: ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERE...
        ^
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES star(star_id) ON DELETE CASCADE;
ALTER TABLE
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES planet(planet_id) ON DELETE CASCADE;
ALTER TABLE
universe=> ALTER TABLE space_mission ADD FOREIGN KEY (target_fk) REFERENCES moon(moon_id) ON DELETE CASCADE;
ALTER TABLE
universe=> DROP TABLE space_mission;
DROP TABLE
universe=> CREATE TABLE space_mission (  space_mission_id SERIAL PRIMARY KEY,  name VARCHAR(100) UNIQUE NOT NULL,  launch_
year INT NOT NULL,  target_type TEXT NOT NULL CHECK (target_type IN ('galaxy', 'star', 'planet', 'moon')),  target_id INT
NOT NULL,  success BOOLEAN DEFAULT TRUE,  description TEXT);
CREATE TABLE
universe=> INSERT INTO space_mission (name, launch_year, target_type, target_id, success, description) VALUES('Apollo 11', 1969, 'moon', 1, TRUE, 'Primera misión tripulada en llegar a la Luna.'),('Voyager 1', 1977, 'planet', 4, TRUE, 'Exploró Júpiter y Saturno antes de salir del sistema solar.'),('Voyager 2', 1977, 'planet', 6, TRUE, 'Visitó Urano y Neptuno, sigue en el espacio interestelar.'),('Hubble Telescope', 1990, 'galaxy', 1, TRUE, 'Observa galaxias lejanas desde la órbita terrestre.'),('Kepler Space Telescope', 2009, 'star', 2, TRUE, 'Descubrió miles de exoplanetas en otras estrellas.'),('Perseverance Rover', 2020, 'planet', 2, TRUE, 'Explora la superficie de Marte buscando signos de vida.'),('New Horizons', 2006, 'planet', 6, TRUE, 'Primera misión en sobrevolar Plutón y el cinturón de Kuiper.'),('James Webb Telescope', 2021, 'galaxy', 1, TRUE, 'Observa las primeras galaxias formadas en el universo.'),('Luna 2', 1959, 'moon', 1, TRUE, 'Primera nave en impactar la superficie de la Luna.'),('Galileo Probe', 1989, 'planet', 4, TRUE, 'Orbitó Júpiter y exploró sus lunas.');
