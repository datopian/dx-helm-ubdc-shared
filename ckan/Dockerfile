FROM viderum/ckan:2.9.9-dev
USER root
COPY setup/prerun.py ${APP_DIR}
COPY setup/supervisor.worker.conf /etc/supervisord.d/worker.conf
COPY setup/start_ckan.sh ${APP_DIR}
RUN chmod +x ${APP_DIR}/start_ckan.sh
RUN pip install --upgrade pip
RUN pip install -r https://raw.githubusercontent.com/shubham-mahajan/ckanext-s3filestore/v1.0.2-r2-ckan-2.9/requirements.txt && \
    pip install -e git+https://github.com/shubham-mahajan/ckanext-s3filestore.git@v1.0.2-r2-ckan-2.9#egg=ckanext-s3filestore && \
    pip install --no-cache-dir -e git+https://github.com/datopian/ckanext-dataexplorer-react.git#egg=ckanext-dataexplorer-react && \
    pip install -r https://raw.githubusercontent.com/datopian/ckanext-visualize/master/requirements.txt && \
    pip install -e git+https://github.com/ckan/ckanext-pages.git#egg=ckanext-pages && \
    pip install -e git+https://github.com/ckan/ckanext-pdfview.git#egg=ckanext-pdfview && \
    pip install cryptography==42.0.7 && \
    pip install --no-cache-dir  -e git+https://github.com/datopian/ckanext-googleanalytics.git@UBDC-project#egg=ckanext-googleanalytics && \
    pip install -r https://raw.githubusercontent.com/datopian/ckanext-googleanalytics/UBDC-project/requirements.txt && \
    pip install -e git+https://github.com/ckan/ckanext-showcase.git@v1.5.2#egg=ckanext-showcase && \
    pip install --no-cache-dir -e git+https://github.com/datopian/ckanext-doi.git@d384e2a9169f3876adff436b0be4f94dfe252431#egg=ckanext-doi && \
    pip install -e git+https://github.com/ckan/ckanext-dcat.git@v1.3.0#egg=ckanext-dcat && \
    pip install -r https://raw.githubusercontent.com/ckan/ckanext-dcat/v1.3.0/requirements.txt && \
    pip install -e git+https://github.com/ckan/ckanext-scheming#egg=ckanext-scheming  && \
    pip install -e git+https://github.com/EnviDat/ckanext-composite.git#egg=ckanext-composite  && \
    pip install -e git+https://github.com/open-data/ckanext-repeating.git#egg=ckanext-repeating  && \
    pip install --no-cache-dir -e git+https://github.com/datopian/ckanext-restricted.git@609ccc7a7ce89a5cfc1849acb2f9a7cde31bb566#egg=ckanext-restricted  && \
    pip install -e git+https://github.com/datopian/ckanext-issues.git#egg=ckanext-issues  && \
    pip install --no-cache-dir -e git+https://github.com/datopian/ckanext-ubdc.git@v1.0.34#egg=ckanext-ubdc && \
    pip install --no-cache-dir -e git+https://github.com/datopian/ckanext-datasetapproval.git#egg=ckanext-datasetapproval


ENV CKAN__PLUGINS envvars stats text_view image_view webpage_view pdf_view resource_proxy s3filestore datastore datapusher dataexplorer_view dataexplorer_table_view dataexplorer_chart_view dataexplorer_map_view restricted pages doi ubdc issues showcase googleanalytics dataset_approval repeating composite scheming_datasets dcat structured_data
RUN ckan config-tool /srv/app/production.ini "ckan.plugins = ${CKAN__PLUGINS}"

# Apply patches
RUN if [ -d "$APP_DIR/patches/ckan" ] ; then rm -rf $APP_DIR/patches/ckan ; fi

COPY patches ${APP_DIR}/patches
RUN for d in $APP_DIR/patches/ckan/*; do \
    for f in `ls $d/*.patch | sort -g`; do \
    cd $SRC_DIR/`basename "$d"` && echo "$0: Applying patch $f to $SRC_DIR/`basename $d`"; patch -p1 < "$f" ; \
    done ; \
    done
     
USER ckan
