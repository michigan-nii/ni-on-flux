# Using the Flux cluster for neuroimaging analysis

The purpose of this presentation is to provide a quick overview of how
to use Flux for neuroimaging analysis.  Flux has been used successfully
to run a variety of neuroimaging analyses in the last several years. Here
is a list of some examples.

* The FreeSurfer `recon-all` command can take anywhere from about 8 hours
  to more 24 hours per subject. One lab found an average of about 12 hours,
  and they had approximately 200 subjects.  That would have been about
  50 days just to do preprocessing; using Flux and running 12 subjects at
  a time enabled them to finish processing in one week.

* Flux has a number of computers with general-purpose graphical processing
  units, typically called GPUs.  Several packages can use these for
  tremendous reductions in processing time.

  For example, FSL&rsquo;s `bedpostx` and `probtrackx` programs have been
  rewritten to use them, and that cuts processing time per subject from 36
  hours to 20 minutes.  One graduate student, using the account provided to
  LSA people at no cost, was able to reduce processing time from an
  estimated 250+ days to a bit less than a day and a half by running two
  subjects at a time on the GPU machines.
  (See the slides from Leigh Goetschius&rsquo;s talk, July 18, 2017.
  &ldquo;<a href="http://www.umich.edu/~nii/events/ProbabilisticTractographyOverview_NII.pptx">Where
  is your white matter going (and how sure can you be)?</a>&rdquo;)

