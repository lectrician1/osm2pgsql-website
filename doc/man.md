---
layout: doc
title: Manual Page
---

<pre style="width: 48em; margin: 0 auto;">
OSM2PGSQL(1)                General Commands Manual               OSM2PGSQL(1)



NAME
       osm2pgsql - Openstreetmap data to PostgreSQL converter.

SYNOPSIS
       osm2pgsql [options] planet.osm
       osm2pgsql [options] planet.osm.{gz,bz2,pbf}
       osm2pgsql [options] file1.osm file2.osm file3.osm

DESCRIPTION
       This manual page documents briefly the osm2pgsql command.

       osm2pgsql  imports  data  from  OSM  file(s) into a PostgreSQL database
       suitable for use by the Mapnik renderer or the Nominatim geocoder.
       OSM  planet  snapshots  can  be  downloaded  from  https://planet.open‐
       streetmap.org/. Partial planet files ("extracts") for various countries
       are available, see https://wiki.openstreetmap.org/wiki/Planet.osm.

       Extracts  in  PBF  (ProtoBufBinary)  format  are  also  available  from
       https://download.geofabrik.de/.

       When  operating  in  "slim"  mode  (and on a database created in "slim"
       mode!), osm2pgsql can  also  process  OSM  change  files  (osc  files),
       thereby bringing an existing database up to date.

OPTIONS
       This  program  follows the usual GNU command line syntax, with long op‐
       tions starting with two dashes (`-').  A summary of options is included
       below.

       -a|--append
              Add  the  OSM  file  into the database without removing existing
              data.

       -b|--bbox
              Apply a bounding box filter on the imported data.  Must be spec‐
              ified     as:     minlon,minlat,maxlon,maxlat     e.g.    --bbox
              -0.5,51.25,0.5,51.75

       -c|--create
              Remove existing data from the database. This is the  default  if
              --append is not specified.

       -d|--database name
              The name of the PostgreSQL database to connect to.

       -i|--tablespace-index tablespacename
              Store  all  indices in a separate PostgreSQL tablespace named by
              this parameter.  This allows one to e.g. store  the  indices  on
              faster storage like SSDs.

        --tablespace-main-data tablespacename
              Store the data tables (non slim) in the given tablespace.

        --tablespace-main-index tablespacename
              Store the indices of the main tables (non slim) in the given ta‐
              blespace.

        --tablespace-slim-data tablespacename
              Store the slim mode tables in the given tablespace.

        --tablespace-slim-index tablespacename
              Store the indices of the slim  mode  tables  in  the  given  ta‐
              blespace.

       -l|--latlong
              Store data in degrees of latitude & longitude.

       -m|--merc
              Store  data in Spherical Mercator (Web Mercator, EPSG:3857) (the
              default).

       -E|--proj num
              Use projection EPSG:num.

       -p|--prefix prefix_string
              Prefix for table names (default: planet_osm).

       -r|--input-reader format
              Select format of the input file. Available choices are auto (de‐
              fault)  for  autodetecting  the  format,  xml for OSM XML format
              files, o5m for o5m formatted files and pbf for  OSM  PBF  binary
              format.

       -s|--slim
              Store  temporary  data  in  the database. Without this mode, all
              temporary data is stored in RAM and if you do  not  have  enough
              the  import  will  not  work  successfully.  With slim mode, you
              should be able to import the data even on a system with  limited
              RAM,  although  if  you do not have enough RAM to cache at least
              all of the nodes, the time to import the  data  will  likely  be
              greatly increased.

         --drop
              Drop  the  slim mode tables from the database once the import is
              complete. This can greatly reduce the size of the  database,  as
              the  slim  mode  tables  typically  are  the  same  size, if not
              slightly bigger than the main tables. It does not, however,  re‐
              duce  the maximum spike of disk usage during import. It can fur‐
              thermore increase the import speed, as no  indices  need  to  be
              created  for the slim mode tables, which (depending on hardware)
              can nearly halve import time. Slim mode tables however  have  to
              be persistent if you want to be able to update your database, as
              these tables are needed for diff processing.

       -S|--style /path/to/style
              Location of the osm2pgsql style file. This specifies which  tags
              from  the data get imported into database columns and which tags
              get dropped. Defaults to /usr/share/osm2pgsql/default.style.

       -C|--cache num
              Only for slim mode: Use up to num many MB  of  RAM  for  caching
              nodes.  Giving  osm2pgsql sufficient cache to store all imported
              nodes typically greatly increases the speed of the import.  Each
              cached  node  requires  8  bytes  of cache, plus about 10% - 30%
              overhead. As a rule of thumb, give a bit more than the  size  of
              the import file in PBF format. If the RAM is not big enough, use
              about 75% of memory. Make sure to leave  enough  RAM  for  Post‐
              greSQL.   It needs at least the amount of `shared_buffers` given
              in its configuration.  Defaults to 800.

         --cache-strategy strategy
              There are a number of different modes in which osm2pgsql can or‐
              ganize  its node cache in RAM. These are optimized for different
              assumptions of the data and the  hardware  resources  available.
              Currently  available  strategies  are dense, chunked, sparse and
              optimized. dense assumes that the node id  numbers  are  densely
              packed,  i.e. only a few IDs in the range are missing / deleted.
              For planet extracts this is usually not  the  case,  making  the
              cache  very inefficient and wasteful of RAM. sparse assumes node
              IDs in the data  are  not  densely  packed,  greatly  increasing
              caching  efficiency  in  these  cases.   If node IDs are densely
              packed, like in the full planet,  this  strategy  has  a  higher
              overhead  for  indexing the cache. optimized uses both dense and
              sparse strategies for different ranges of the  ID  space.  On  a
              block  by block basis it tries to determine if it is more effec‐
              tive to store the block of IDs in sparse or dense mode. This  is
              the default and should be typically used.

       -U|--username name
              Postgresql user name.

       -W|--password
              Force password prompt.

       -H|--host hostname
              Database server hostname or socket location.

       -P|--port num
              Database server port.

       -e|--expire-tiles [min_zoom-]max-zoom
              Create a tile expiry list.

       -o|--expire-output /path/to/expire.list
              Output file name for expired tiles list.

       -O|--output
              Specifies  the  output  back-end or database schema to use. Cur‐
              rently osm2pgsql supports pgsql, flex, gazetteer and null. pgsql
              is  the  default  output  back-end / schema and is optimized for
              rendering with Mapnik.  gazetteer is a db schema  optimized  for
              geocoding and is used by Nominatim.  The experimental flex back‐
              end allows more flexible configuration.  null does not write any
              output  and is only useful for testing or with --slim for creat‐
              ing slim tables. There is also a multi backend. This is now dep‐
              recated and will be removed in future versions of osm2pgsql.

       -x|--extra-attributes
              Include  attributes  for  each object in the database.  This in‐
              cludes the username, userid, timestamp and version.  Note:  this
              option also requires additional entries in your style file.

       -k|--hstore
              Add tags without column to an additional hstore (key/value) col‐
              umn to PostgreSQL tables.

       -j|--hstore-all
              Add all tags to an additional hstore (key/value) column in Post‐
              greSQL tables.

       -z|--hstore-column key_name
              Add  an additional hstore (key/value) column containing all tags
              that start with the specified string, eg --hstore-column "name:"
              will  produce  an  extra hstore column that contains all name:xx
              tags

         --hstore-match-only
              Only keep objects that have a value in one of the columns  (nor‐
              mal action with --hstore is to keep all objects).

         --hstore-add-index
              Create indices for the hstore columns during import.

       -G|--multi-geometry
              Normally  osm2pgsql  splits  multi-part geometries into separate
              database rows per part.  A single OSM id can therefore have sev‐
              eral  rows.  With  this  option,  PostgreSQL  instead  generates
              multi-geometry features in the PostgreSQL tables.

       -K|--keep-coastlines
              Keep coastline data rather than filtering it  out.   By  default
              natural=coastline tagged data will be discarded based on the as‐
              sumption that Shapefiles generated by OSMCoastline (https://osm‐
              data.openstreetmap.de/) will be used.

         --number-processes num
              Specifies  the number of parallel processes used for certain op‐
              erations. If disks are fast enough e.g. if you have an SSD, then
              this can greatly increase speed of the "going over pending ways"
              and "going  over  pending  relations"  stages  on  a  multi-core
              server.

       -I|--disable-parallel-indexing
              By  default osm2pgsql initiates the index building on all tables
              in parallel to increase performance. This can  be  disadvantages
              on slow disks, or if you don't have enough RAM for PostgreSQL to
              perform up to 7 parallel index building processes (e.g.  because
              maintenance_work_mem is set high).

         --flat-nodes /path/to/nodes.cache
              The flat-nodes mode is a separate method to store slim mode node
              information on disk.  Instead of storing this information in the
              main  PostgreSQL  database,  this  mode creates its own separate
              custom database to store the information. As this  custom  data‐
              base has application level knowledge about the data to store and
              is not general purpose, it can store the data  much  more  effi‐
              ciently.  Storing  the  node information for the full planet re‐
              quires more than 300GB in PostgreSQL, the same data is stored in
              "only"  50GB  using  the flat-nodes mode. This can also increase
              the speed of applying diff  files.  This  option  activates  the
              flat-nodes mode and specifies the location of the database file.
              It is a single large file. This mode  is  only  recommended  for
              full  planet imports as it doesn't work well with small imports.
              The default is disabled.

       -h|--help
              Help information.
              Add -v to display supported projections.

       -v|--verbose
              Verbose output.

SUPPORTED PROJECTIONS
       Latlong             (-l) SRS:  4326 (none)
       Spherical Mercator   (-m)  SRS:3857  +proj=merc  +a=6378137  +b=6378137
       +lat_ts=0.0  +lon_0=0.0 +x_0=0.0 +y_0=0 +k=1.0 +units=m +nadgrids=@null
       +no_defs +over
       EPSG-defined        (-E) SRS: +init=epsg:(as given in parameter)

SEE ALSO
       proj(1), postgres(1).  osmcoastline(1).

AUTHOR
       osm2pgsql was written by Jon Burgess, Artem Pavlenko, and  other  Open‐
       StreetMap project members.

       This  manual  page was written by Andreas Putzo <andreas@putzo.net> for
       the Debian project, and amended by OpenStreetMap authors.

</pre>