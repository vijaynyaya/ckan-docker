FROM ckan/ckan-base:2.10.4

# Install any extensions needed by your CKAN instance
# See Dockerfile.dev for more details and examples

## Scheming ###
RUN  pip3 install -e 'git+https://github.com/ckan/ckanext-scheming.git@master#egg=ckanext-scheming'

## DCAT ###
RUN  pip3 install -e git+https://github.com/ckan/ckanext-dcat.git@v1.7.0#egg=ckanext-dcat && \
    pip3 install -r https://raw.githubusercontent.com/ckan/ckanext-dcat/v1.7.0/requirements.txt

# Clone the extension(s) your are writing for your own project in the `src` folder
# to get them mounted in this image at runtime

## Datopian ###
RUN  pip3 install -e git+https://github.com/datopian/ckanext-noanonaccess.git#egg=ckanext-noanonaccess
RUN  pip3 install -e git+https://github.com/datopian/ckanext-s3filestore.git@b7789ff905c854347f460ee506dfe7ae7f228ac1#egg=ckanext-s3filestore


# Copy custom initialization scripts
COPY docker-entrypoint.d/* /docker-entrypoint.d/

# Apply any patches needed to CKAN core or any of the built extensions (not the
# runtime mounted ones)
COPY patches ${APP_DIR}/patches

RUN for d in $APP_DIR/patches/*; do \
        if [ -d $d ]; then \
            for f in `ls $d/*.patch | sort -g`; do \
                cd $SRC_DIR/`basename "$d"` && echo "$0: Applying patch $f to $SRC_DIR/`basename $d`"; patch -p1 < "$f" ; \
            done ; \
        fi ; \
    done
