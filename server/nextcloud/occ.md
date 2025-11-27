# Using occ - for example this adds missing inices

/var/www/html/nextcloud# sudo -u www-data php8.3 occ db:add-missing-indices

## Maintanence window set to 2am 

sudo -u www-data php8.3 occ config:system:set maintenance_window_start --type=integer --value=2
