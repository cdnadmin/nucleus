LIBUV= ../../deps/libuv

# Uncomment the following to make a static binary
# Make sure to `make distclean` before building when changing CC.
# CC= musl-gcc -Os -static

nucleus: main.o miniz.o duktape.o duv/duv.a ${LIBUV}/.libs/libuv.a
	${CC} $^ -lm -lpthread -o $@

test: test-dir test-zip test-app test-app-tiny

test-dir: nucleus
	./nucleus ../../app -- 1 2 3

test-zip: nucleus app.zip
	./nucleus app.zip -- 4 5 6

test-app: app
	./app 7 8 9

test-app-tiny: app-tiny
	./app-tiny 10 11 12

app: app.zip nucleus
	cat nucleus app.zip > app
	chmod +x app

app-tiny: app.zip prefix
	cat prefix app.zip > app-tiny
	chmod +x app-tiny

prefix: nucleus
	echo "#!$(shell pwd)/nucleus --" > prefix

app.zip: ../../app* ../../app/deps/*
	rm -f app.zip
	cd ../../app; zip -9 -r ../implementations/duktape/app.zip .; cd -

main.o: main.c
	${CC} -std=c99 -Wall -Wextra -pedantic -Werror -c $< -o $@


${LIBUV}/.libs/libuv.a: ${LIBUV}/Makefile
	${MAKE} -C ${LIBUV}

${LIBUV}/Makefile: ${LIBUV}/configure
	cd ${LIBUV}; ./configure; cd -

${LIBUV}/configure: ${LIBUV}/autogen.sh
	cd ${LIBUV}; ./autogen.sh; cd -

duv/duv.a: duv/*.c duv/*.h
	${MAKE} -C duv

duktape.o: duktape-releases/src/duktape.c duktape-releases/src/duktape.h
	${CC} -std=c99 -Wall -Wextra -pedantic -c $< -o $@

miniz.o: ../../deps/miniz.c
	${CC} -std=gnu99 -c $< -o $@

clean:
	rm -f nucleus *.o app.zip app prefix app-tiny
	${MAKE} -C duv clean

distclean: clean
	${MAKE} -C ${LIBUV} distclean
