# To create new ASCII data files:
# Change version numbers VerHr and VerMN as appropriate

# Data version numbers - will be over-ridden by command line arguments.
VerHr=$$(date +%Y%m)
VerMn=$$(date +%Y%m)

TIMESTAMP=00000000

program=MagmodelinputONE_corrected2

# Dir for downloaded OMNI data from NSSDC ftp site
DIRTMP=~/tmp/QinDenton/tmp

# Dir for local copies of ASCII input and output data
DATADIR=~/tmp/QinDenton/io

# Dir for finall copies of ASCII input and output data
MERGEDIR=./merged

dirs:
	- mkdir -p $(DIRTMP)
	- mkdir -p $(DATADIR)
	- mkdir -p $(MERGEDIR)

allhour:
	make dirs
	make $(program)
#	make ishourupdateneeded
	make omnikpdst
	make omniupdatehour
	make hourall
	make mergekpdst
	make mergehour
	make mergetoftphour
	make symlinkhour

all5min:
	make dirs
	make $(program)
	make omnikpdst
	make omniupdate5min
	make 5minall
	make mergekpdst
	make merge5min
	make mergetoftp5min
	make symlink5min

all1min:
	make dirs
#	make clean1min
	make $(program)
#	make is1minupdateneeded # Not implemented
	make omnikpdst
	make omniupdate1min
	make 1minall
	make mergekpdst
	make merge1min
	make mergetoftp1min
	make symlink1min

all:
	make clean1min
	make $(program)
	make omniupdate	
	make hourall
	make 5minall
	make 1minall
	make merge
	make mergetoftp
	make symlink

MagmodelinputONE: MagmodelinputONE.f
	gfortran -o MagmodelinputONE MagmodelinputONE.f

# See README-rsw.txt for explanation of _corrected.f file
MagmodelinputONE_corrected: MagmodelinputONE_corrected.f
	gfortran -o MagmodelinputONE_corrected MagmodelinputONE_corrected.f

# See README-rsw.txt for explanation of _corrected2.f file
MagmodelinputONE_corrected2: MagmodelinputONE_corrected2.f
	gfortran -o MagmodelinputONE_corrected2 MagmodelinputONE_corrected2.f

mergekpdst: 5min/kpdst.lst
	zip -j $(DIRTMP)/kpdst.lst.zip ./5min/kpdst.lst

merge1min: 1min/omni_min.asc 
	mkdir -p $(DIRTMP)
	zip -j $(DIRTMP)/omni_min-v$(VerMn).asc.zip ./1min/omni_min.asc
	zip -j $(DIRTMP)/WGparametersmin-v$(VerMn).d.zip ./1min/WGparametersmin.d

merge5min: 5min/omni_5min.asc 
	mkdir -p $(DIRTMP)
	zip -j $(DIRTMP)/omni_5min-v$(VerMn).asc.zip ./5min/omni_5min.asc
	zip -j $(DIRTMP)/WGparameters5min-v$(VerMn).d.zip ./5min/WGparameters5min.d

mergehour: hour/omni2_hour.dat
	mkdir -p $(DIRTMP)
	zip -j $(DIRTMP)/omni2_hour-v$(VerHr).dat.zip ./hour/omni2_hour.dat
	zip -j $(DIRTMP)/WGhour-v$(VerHr).d.zip ./hour/WGhour.d

mergetoftphour:
	mkdir -p $(MERGEDIR)/hour/merged
	mv $(DIRTMP)/omni2_hour-v$(VerHr).dat.zip $(MERGEDIR)/hour/merged
	mv $(DIRTMP)/WGhour-v$(VerHr).d.zip $(MERGEDIR)/hour/merged
	cp ./hour/WGhour.d $(MERGEDIR)/hour/merged/WGhour-v$(VerHr).d

mergetoftp5min:
	mkdir -p $(MERGEDIR)/5min/merged
	mv $(DIRTMP)/omni_5min-v$(VerMn).asc.zip $(MERGEDIR)/5min/merged
	mv $(DIRTMP)/WGparameters5min-v$(VerMn).d.zip $(MERGEDIR)/5min/merged
	cp $(DIRTMP)/kpdst.lst.zip ./5min/kpdst.lst $(MERGEDIR)/5min/merged

mergetoftp1min:
	mkdir -p $(MERGEDIR)/1min/merged
	mv $(DIRTMP)/omni_min-v$(VerMn).asc.zip $(MERGEDIR)/1min/merged
	mv $(DIRTMP)/WGparametersmin-v$(VerMn).d.zip $(MERGEDIR)/1min/merged
	cp $(DIRTMP)/kpdst.lst.zip ./1min/kpdst.lst $(MERGEDIR)/1min/merged

rsynchour:
	rsync -avz merged/hour weigel@mag.gmu.edu:/var/www/ftp/QinDenton

rsync5min:
	rsync -avz merged/5min weigel@mag.gmu.edu:/var/www/ftp/QinDenton

symlinkhour:
	mkdir -p $(MERGEDIR)/hour/merged/latest
	rm -f $(MERGEDIR)/hour/merged/latest/*
	cd $(MERGEDIR)/hour/merged/latest; \
	 ln -s ../omni2_hour-v$(VerHr).dat.zip omni2_hour-latest.dat.zip
	cd $(MERGEDIR)/hour/merged/latest; \
	 ln -s ../WGhour-v$(VerHr).d.zip WGhour-latest.d.zip
	cd $(MERGEDIR)/hour/merged/latest; \
	 ln -s ../WGhour-v$(VerHr).d WGhour-latest.d

#	cd $(MERGEDIR)/hour/merged/latest; \
#	 ln -s ../QinDenton_hour_merged_$(TIMESTAMP)-v$(VerHr).cdf.zip QinDenton_hour-latest.cdf.zip
#	cd $(MERGEDIR)/hour/merged/latest; \
#	 ln -s ../QinDenton_hour_merged_$(TIMESTAMP)-v$(VerHr).cdf QinDenton_hour-latest.cdf
#	cd $(MERGEDIR)/hour/merged/latest; \
#	 ln -s ../QinDenton_hour_merged_$(TIMESTAMP)-v$(VerHr).mat.zip QinDenton_hour-latest.mat.zip
#	cd $(MERGEDIR)/hour/merged/latest; \
#	 ln -s ../QinDenton_hour_merged_$(TIMESTAMP)-v$(VerHr).mat QinDenton_hour-latest.mat

symlink5min:
	mkdir -p $(MERGEDIR)/5min/merged/latest
	rm -f $(MERGEDIR)/5min/merged/latest/*
	cd $(MERGEDIR)/5min/merged/latest; \
	 ln -s ../WGparameters5min-v$(VerMn).d.zip WGparameters5min-latest.d.zip
	cd $(MERGEDIR)/5min/merged/latest; \
	 ln -s ../kpdst.lst.zip kpdst-latest.lst.zip
	cd $(MERGEDIR)/5min/merged/latest; \
	 ln -s ../omni_5min-v$(VerMn).asc.zip omni_5min-latest.asc.zip

#	cd $(MERGEDIR)/5min/merged/latest; \
#	 ln -s ../QinDenton_5min_merged_$(TIMESTAMP)-v$(VerMn).cdf.zip QinDenton_5min-latest.cdf.zip
#	cd $(MERGEDIR)/5min/merged/latest; \
#	 ln -s ../QinDenton_5min_merged_$(TIMESTAMP)-v$(VerMn).cdf QinDenton_5min-latest.cdf
#	cd $(MERGEDIR)/5min/merged/latest; \
#	 ln -s ../QinDenton_5min_merged_$(TIMESTAMP)-v$(VerMn).mat.zip QinDenton_5min-latest.mat.zip
#	cd $(MERGEDIR)/5min/merged/latest; \
#	 ln -s ../QinDenton_5min_merged_$(TIMESTAMP)-v$(VerMn).mat QinDenton_5min-latest.mat

symlink1min:
	mkdir -p $(MERGEDIR)/1min/merged/latest
	rm -f $(MERGEDIR)/1min/merged/latest/*
	cd $(MERGEDIR)/1min/merged/latest; \
	 ln -s ../WGparametersmin-v$(VerMn).d.zip WGparametersmin-latest.d.zip
	cd $(MERGEDIR)/1min/merged/latest; \
	 ln -s ../omni_min-v$(VerMn).asc.zip omni_min-latest.asc.zip
#	cd $(MERGEDIR)/1min/merged/latest; \
#	 ln -s ../QinDenton_1min_merged_$(TIMESTAMP)-v$(VerMn).cdf.zip QinDenton_1min-latest.cdf.zip
#	cd $(MERGEDIR)/1min/merged/latest; \
#	 ln -s ../QinDenton_1min_merged_$(TIMESTAMP)-v$(VerMn).cdf QinDenton_1min-latest.cdf
#	cd $(MERGEDIR)/1min/merged/latest; \
#	 ln -s ../QinDenton_1min_merged_$(TIMESTAMP)-v$(VerMn).mat.zip QinDenton_1min-latest.mat.zip
#	cd $(MERGEDIR)/1min/merged/latest; \
#	 ln -s ../QinDenton_1min_merged_$(TIMESTAMP)-v$(VerMn).mat QinDenton_1min-latest.mat

omnikpdst:
	make 5min/kpdst.lst

omniupdatehour:
	make hour/omni2_hour.dat

omniupdate5min:
	make 5min/omni_5min.asc

omniupdate1min:
	make 1min/omni_min.asc


###############################################################################
# Hourly
###############################################################################

# Download the full hourly file
# Processing will stop if there are differences
hour/omni2_hour.dat:
	- cd hour; ls -l omni2_all_years.dat > ls-l-last.txt
	cd hour; wget -m -l1 --cut-dirs=4 -nH https://spdf.gsfc.nasa.gov/pub/data/omni/low_res_omni/omni2_all_years.dat
	cd hour; ln -s -f omni2_all_years.dat omni2_hour.dat
	cd hour; ls -l omni2_all_years.dat > ls-l-now.txt

ishourupdateneeded: hour/omni2_hour.dat
	diff hour/ls-l-last.txt hour/ls-l-now.txt; if [ $$? -eq 0 ] ; then echo "No differences in omni2_all_years.dat.  Aborting update.";exit 1 ; fi
	echo "omni2_all_years.dat has changed.  Continuing."
	cp hour/ls-l-now.txt hour/ls-l-last.txt

hourall: MagmodelinputONE_corrected hour/omni2_hour.dat
	echo "60" | ./$(program)
	mkdir -p $(DATADIR)/hour-v$(VerHr)
	cp hour/WGhour.d $(DATADIR)/hour-v$(VerHr)/WGhour.d
	cp hour/omni2_hour.dat $(DATADIR)/hour-v$(VerHr)/omni2_all_years.dat
###############################################################################

###############################################################################
# 1min and 5min
###############################################################################
5min/kpdst.lst: hour/omni2_hour.dat
	cut -c 1-11,219-221,226-231 hour/omni2_hour.dat > tmp.dat
	more +280513 tmp.dat > 5min/kpdst.lst
	rm -f tmp.dat

1min/kpdst.lst: 5min/kpdst.lst
	cd 1min ; cp ../5min/kpdst.lst .

5min/omni_5min.asc:
	mkdir -p $(DIRTMP)
	cd $(DIRTMP); wget -m -l1 https://spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni
#	find $(DIRTMP)/spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni/ -name "omni_5min*.asc" | sort | xargs -i cat {} > 5min/omni_5min.asc	
	find $(DIRTMP)/spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni/ -name "omni_5min*.asc" | grep -v -e "199[0-4]" | grep -v -e "19[6-8]" | sort | xargs -i cat {} > 5min/omni_5min.asc	

1min/omni_min.asc:
	mkdir -p $(DIRTMP)
	cd $(DIRTMP); wget -m -l1 https://spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni
#	find $(DIRTMP)/spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni/ -name "omni_min*.asc" | grep "sort | xargs -i cat {} > 1min/omni_min.asc	
	find $(DIRTMP)/spdf.gsfc.nasa.gov/pub/data/omni/high_res_omni/ -name "omni_min*.asc" | grep -v -e "199[0-5]" | grep -v -e "19[6-8]" | sort | xargs -i cat {} > 1min/omni_min.asc	

5minall: 5min/kpdst.lst 5min/omni_5min.asc
	echo "5" | ./$(program)
	mkdir -p $(DATADIR)/5min-v$(VerMn)
	cp 5min/WGparameters5min.d $(DATADIR)/5min-v$(VerMn)/WGparameters5min.d
	cp 5min/omni_5min.asc $(DATADIR)/5min-v$(VerMn)/omni_5min.asc

1minall: 1min/kpdst.lst 1min/omni_min.asc
	echo "1" | ./$(program)
	mkdir -p $(DATADIR)/1min-v$(VerMn)
	cp 1min/WGparametersmin.d $(DATADIR)/1min-v$(VerMn)/WGparametersmin.d
	cp 1min/omni_min.asc $(DATADIR)/1min-v$(VerMn)/omni_min.asc
###############################################################################

###############################################################################
# Tests
#
# Compare sample output from Denton.  Differences are probably due to
# the fact that the omni2_2000.dat file changed (?!) at SPDF between the
# time Denton downloaded it and the time it was downloaded
# and copied to ./hour (a year or so later).
testhour2000: MagmodelinputONE
	echo "60" | ./MagmodelinputONE
	diff ../MagParameterProgram/hour/WGhour_0.d ./hour/WGhour.d	

testhour: MagmodelinputONE
	echo "60" | ./MagmodelinputONE

testhour_corrected: MagmodelinputONE_corrected
	echo "60" | ./MagmodelinputONE_corrected

test5min: MagmodelinputONE
	- gunzip ../MagParameterProgram/5min/kpdst.lst.gz
	- gunzip ../MagParameterProgram/5min/omni_5min.asc.gz
	echo "5" | ./MagmodelinputONE

test1min: MagmodelinputONE
	- gunzip ../MagParameterProgram/1min/kpdst.lst.gz
	- gunzip ../MagParameterProgram/1min/omni_min.asc.gz
	echo "1" | ./MagmodelinputONE
###############################################################################

clean:
	- rm -f hour/*.*
	- rm -f 5min/*.*
	- rm -f 1min/*.*
	- rm -f *~
	- rm -f *.o

distclean:
	make clean
	rm -rf $(DIRTMP)
	rm -rf $(DATADIR)
	- rm -f MagmodelinputONE
	- rm -f MagmodelinputONE_corrected
	- rm -f MagmodelinputONE_corrected2

