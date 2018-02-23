FROM terapyon/plone.docker


COPY site.cfg /plone/instance/
RUN bin/buildout -c site.cfg
