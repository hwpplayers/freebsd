# $FreeBSD$

.PATH: ${.CURDIR:H}/common

SRCS=		crti.S crtn.S
OBJS=		${SRCS:N*.h:R:S/$/.o/g}
OBJS+=		Scrt1.o crt1.o gcrt1.o
CFLAGS+=	-I${.CURDIR:H}/common \
		-I${SRCTOP}/lib/libc/include
CFLAGS+=	-DCRT_IRELOC_SUPPRESS

FILES=		${OBJS}
FILESMODE=	${LIBMODE}
FILESOWN=	${LIBOWN}
FILESGRP=	${LIBGRP}
FILESDIR=	${LIBDIR}
# These FILES qualify as libraries for the purpose of LIBRARIES_ONLY.
.undef LIBRARIES_ONLY

CLEANFILES=	${OBJS} crt1_c.o crt1_s.o gcrt1_c.o Scrt1_c.o

crt1_c.o: crt1_c.c
	${CC} ${CFLAGS} ${STATIC_CFLAGS} -c -o ${.TARGET} ${.CURDIR}/crt1_c.c

crt1.o:	crt1_c.o crt1_s.o
	${LD} ${_LDFLAGS} -o crt1.o -r crt1_s.o crt1_c.o

gcrt1_c.o: crt1_c.c
	${CC} ${CFLAGS} ${STATIC_CFLAGS} -DGCRT -c -o ${.TARGET} ${.CURDIR}/crt1_c.c

gcrt1.o: gcrt1_c.o crt1_s.o
	${LD} ${_LDFLAGS} -o gcrt1.o -r crt1_s.o gcrt1_c.o

Scrt1_c.o: crt1_c.c
	${CC} ${CFLAGS} -fPIC -DPIC -c -o ${.TARGET} ${.CURDIR}/crt1_c.c

Scrt1.o: Scrt1_c.o crt1_s.o
	${LD} ${_LDFLAGS} -o Scrt1.o -r crt1_s.o Scrt1_c.o

.include <bsd.lib.mk>
