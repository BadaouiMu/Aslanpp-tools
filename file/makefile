
# Programmes nÃ©cessaires pour l'installation
# Copyright Yannick Chevalier, <yannick.chevalier@irit.fr>
# Utilisable seulement pour l'enseignement
# Modifié par Mustapha BADAOUI 10 jan 2024, <mustapha.badaoui@univ-tlse3.fr>
JAR:=jar
GIT:=git
MAVEN:=mvn
OCAML:=ocamlrn
CL_ATSE_VERSION:=2.5-26_bin_i686-windows-mac_2014-12-20
CL_ATSE_DL:=http://www.irit.fr/~Yannick.Chevalier/Cours/cl-atse_$(CL_ATSE_VERSION).tgz
# note: - ofmc 2016 does not compile
#       - ofmc 2014 accepts only Avispa's input
#	 on most files (no hc support)
BACKEND:=cl-atse
DIR_TREE:=bin result aslan aslanpp git
# barely useful variables
ASLANPP_VERSION:=1.4.9

.precious: % aslan/%.aslan result/%
.PHONY: setup
setup: $(DIR_TREE) bin/ofmc bin/cl-atse bin/aslanpp-connector.jar library

$(DIR_TREE): %:
	mkdir -p $@


bin/ofmc:  git/ofmc-2014/src/ofmc
	cp -u $< $@

git/ofmc-2014/src/ofmc: git/ofmc-2014
	make -C git/ofmc-2014/src ofmc

git/ofmc-2014: git/ofmc-2014.zip
	if [ ! -d $@ ] ; then  cd git && unzip ofmc-2014.zip && \rm -rf __MACOSX ; fi

git/ofmc-2014.zip:
	if [ ! -e $@ ] ; then \
		cd git && wget http://www.imm.dtu.dk/~samo/ofmc-2014.zip ; \
	fi

git/cl-atse:
	[[ -d "$@" ]] || mkdir -p "$@"

git/cl-atse/cl-atse_$(CL_ATSE_VERSION).tgz: git/cl-atse
	if [ ! -e $@ ] ; then \
		cd git/cl-atse && wget --no-check-certificate $(CL_ATSE_DL) ; \
	fi

git/cl-atse/cl-atse_bytecode-OCaml: git/cl-atse/cl-atse_2.5-26_bin_i686-windows-mac_2014-12-20.tgz
	cd git/cl-atse ; \
	tar xvf cl-atse_2.5-26_bin_i686-windows-mac_2014-12-20.tgz

bin/cl-atse: git/cl-atse/cl-atse_bytecode-OCaml
	cp -u $< $@

git/ASLanPPConnector: git
	mkdir -p git
	cd git && $(GIT) clone https://github.com/siemens/ASLanPPConnector.git

git/ASLanPPConnector/binaries/aslanpp-connector.jar: git/ASLanPPConnector/src/aslanpp-translator/compiled
	cp -u git/ASLanPPConnector/src/aslanpp-translator/target/aslanpp-translator-$(ASLANPP_VERSION)-jar-with-dependencies.jar $@

git/ASLanPPConnector/src/aslanpp-translator/compiled:
	cd git/ASLanPPConnector/src/ ; \
	./build.sh
	cd git/ASLanPPConnector/src/aslanpp-translator/
	touch $@

library: aslan aslanpp
	cd aslanpp ; \
	wget -O- https://web.archive.org/web/20160828151412/http://www.avantssar.eu/?page_id=32 | grep testCases | \
	sed -e 's|<a href="||g' -e 's|">.*||' -e 's|<p>||'  | \
	grep http | wget -i- ; \
	mv *.aslan ../aslan

bin/aslanpp-connector.jar: git/ASLanPPConnector git/ASLanPPConnector/binaries/aslanpp-connector.jar
	cp -u git/ASLanPPConnector/binaries/aslanpp-connector.jar $@

aslan/%.aslan: aslanpp/%.aslan++ #Â bin/aslanpp-connector.jar
	java -jar ./bin/aslanpp-connector.jar -gas -o $@ $<

result/%.aslan.result: aslan/%.aslan #Â bin/$(BACKEND)
	./bin/$(BACKEND) $< > $@

result/%.result: result/%.aslan.result aslan/%.aslan
	java -jar ./bin/aslanpp-connector.jar -o $@ -ar $< aslan/$*.aslan