# This example shows that volumes are not maintained across build steps, so be careful if you generate files in a volume, you won't be able to access them later
# this can catch one off guard with multi-stage builds if a volume is there that you don't know about
# make sure to run use --no-cache or some output will be cached from previous runs and won't show and will be confusing

FROM alpine 

VOLUME /vol

RUN echo world > /vol/hello.txt \
    && cat /vol/hello.txt

RUN echo world-not-in-volume > /hello.txt \
    && cat /hello.txt

# this will print out
RUN cat /hello.txt

# oddity - copy can modify volume after created, unlike run
# I imagine that with copy, the container isn't run so no volume is mounted and we're just modifying the container file system - I need to investigate
# https://github.com/moby/moby/issues/21728
COPY hello-to-copy.txt /vol/.
RUN cat /vol/hello-to-copy.txt

# volumes are not maintained across build instructions (separate containers)
# so this instruction will fail
# run can't modify volumes - data lost
RUN cat /vol/hello.txt

