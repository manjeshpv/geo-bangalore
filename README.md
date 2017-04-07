# geo-bangalore


References:

https://gist.github.com/mindplay-dk/4755200#file-cdh_state_codes-txt

http://censusindia.gov.in/Tables_Published/Admin_Units/admin.html

```mysql
# 8 continent
INSERT INTO place (name, type, ref_id) SELECT
                                         name,
                                         'continent',
                                         id
                                       FROM continents;

# 249 country
INSERT INTO place (name, type, refId, parentId) SELECT
                                                  cdh_country_codes.country_name AS name,
                                                  'country',
                                                  cdh_country_codes.id,
                                                  place.id                       AS parentId
                                                FROM cdh_country_codes
                                                  LEFT JOIN place ON place.name = cdh_country_codes.un_region;

# 4898 state - global
INSERT
INTO place (name, type, refId, parentId) SELECT
                                           cdh_state_codes.subdiv_name AS NAME,
                                           'state',
                                           cdh_state_codes.id,
                                           place.id                    AS parentId
                                         FROM cdh_state_codes
                                           LEFT JOIN place ON place.name = cdh_state_codes.country_name
                                         WHERE place.type = 'country';

# 645 Districts - India
INSERT
INTO place (name, type, refId, parentId) SELECT
                                           districts.name,
                                           'district',
                                           districts.id   AS refId,
                                           states.placeId AS parentId
                                         FROM `districts`
                                           LEFT JOIN states ON states.id = districts.state_id;

# 5564 taluks
INSERT
INTO place (name, type, refId, parentId) SELECT
                                           taluks.taluk_name,
                                           'taluk',
                                           taluks.id                  AS refId,
                                           ind_districts.adistrict_id AS parentId
                                         FROM `taluks`
                                           JOIN ind_districts ON ind_districts.district_id = taluks.district_id AND
                                                                 taluks.state_id = ind_districts.state_id
                                         WHERE taluks.taluk_id != 0;

#
INSERT INTO place (name, type, refId, parentId) SELECT
                                                  name,
                                                  'area',
                                                  id,
                                                  1947
                                                FROM areas


```
