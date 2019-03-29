# list config
npm config ls -l

# override default config
npm config --global set userconfig /vagrant/npm/.npmrc

# remove config override
npm config --global rm userconfig