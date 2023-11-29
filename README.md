# measure-intensity-of-curved-microtubule-in-Fiji

#This code is for measuring flourescence intensity in curved selection region of filaments in Fiji. Open Fiji-ImageJ, select curved part of interests and run following code in Plugins >> New >> Macro

     macro "Stack profile Data" {
     setBatchMode(true);

     run("Plot Profile");
     Plot.getValues(x, y);
     run("Clear Results");
     for (i=0; i<x.length; i++)
         setResult("x", i, x[i]);
     close();

     n = nSlices;
     for (slice=1; slice<=n; slice++) {
         showProgress(slice, n);
         setSlice(slice);
         profile = getProfile();
         sliceLabel = toString(slice);
         sliceData = split(getMetadata("Label"),"\n");
         if (sliceData.length>0) {
             line0 = sliceData[0];
             if (lengthOf(sliceLabel) > 0)
                 sliceLabel = sliceLabel+ " ("+ line0 + ")";
         }
         for (i=0; i<profile.length; i++)
             setResult(sliceLabel, i, profile[i]);
     }
     setBatchMode(false);
     updateResults;
     }
