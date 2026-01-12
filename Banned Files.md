need to include a catchall for each user, otherwise, after evaluating the user's entires, it'll go an do the domain-wide ones too!!!!


Banned Files:

%banned_rules = (
  'NO-MS-EXEC'=> new_RE( qr'^\.(exe-ms)$' ),
  'PASSALL'   => new_RE( [qr'^' => 0] ),
  'NO-ENCRYPT' => new_RE( qr'.\.(UNDECIPHERABLE)$'i, ),

  'ALLOW_EXE' =>  # pass executables except if name ends in .vbs .pif .scr .bat
    new_RE( qr'.\.(vbs|pif|scr|bat)$'i, [qr'^\.exe$' => 0] ),
  'ALLOW_VBS' =>  # allow names ending in .vbs
    new_RE( [qr'.\.vbs$' => 0] ),
  'NO-VIDEO' => new_RE( qr'^\.movie$',
    qr'.\.(asf|asx|mpg|mpe|mpeg|avi|mp3|wav|wma|wmf|wmv|mov|vob)$'i, ),
  'NO-MOVIES' => new_RE( qr'^\.movie$', qr'.\.(mpg|avi|mov)$'i, ),
  'MYNETS-DEFAULT' => new_RE(
    [ qr'^\.(rpm|cpio|tar)$' => 0 ],  # allow any in Unix-type archives
    qr'.\.(vbs|pif|scr)$'i,           # banned extension - rudimentary
    qr'.\.(exe|vbs|pif|scr|bat|cmd|com|cpl)$'i, # banned extension - basic
    qr'^\.(exe-ms)$',                 # banned file(1) types
  ),
  'DEFAULT' => $banned_filename_re,
);



@banned_filename_maps = (
  { 'user1@example.com' => 'NO-MS-EXEC,PASSALL',
    'user2@example.com' => 'ALLOW_EXE',
    '.subdomain.example.com' => 'ALLOW_VBS',
    'user4@example.com' => 'ALLOW_VBS,ALLOW_EXE',
    '.' => 'DEFAULT',
  },


  'NO-ENCRYPT' => new_RE( qr'.\.(UNDECIPHERABLE)$'i, ),


block exectuables
  '0exe' => new_RE( qr'^\.(exe-ms)$', qr'.\.(bat|chm|class|cmd|com|cpl|dll|exe|ins|iw|pif|scr|vbs)$'i ),

Block dbl extensions
  '0dbl' => new_RE ( qr'\.[^./]*[A-Za-z][^./]*\.(bat|cmd|com|cpl|dll|exe|ins|iw|pif|scr|vbs)\.?$'i )
  
Block Compressed 
  '0cmp' => new_RE ( qr'.\.(arj|bz|bz2|cab|gz|gzip|hex|hqx|jar|lah|lzh|rar|rpm|sea|sit|tar|tgz|z|zip|zoo)$'i)
  
Block PDF
  '0pdf' => new_RE ( qr'.\.(abf|atm|awe|fdf|ofm|p65|pdd|pdf)$'i)

Block Office
  '0off' => new_RE ( qr'.\.(cpr|cwk|cws|dcx|doc|dot|fax|fp|fp3|frm|gim|gix|gna|gnx|gra|mcw|mdb|mdn|met|mpp|obd|pdf|pps|ppt|pre|prs|rtf|shb|shw|wb1|wb2|wdb|wk1|wk3|wk4|wks|wp|wp4|wp5|wp6|wpd|wps|wpt|wpw|wq1|wq2|wri|ws1|ws2|ws3|ws4|ws5|ws6|ws7|wsd|xls|xlt)$'i)

block Video
  '0mov' => new_RE( qr'^\.movie$',  	qr'.\.(asf|asx|avi|cfb|cmv|dir|gal|m3d|mmm|mov|mpe|mpeg|mpg|mvb|qt|qtm|vob|xtp|xy3|xy4|xyp|xyw)$'i, ),

block Music
  '0mus' => new_RE( qr'.\.(aif|aiff|ams|asf|cda|dcr|dsm|idd|it|mdl|med|mid|mod|mp3|mtm|mus|nsa|ra|ram|rm|rmi|rtm|s3m|snd|stm|svx|ult|voc|wav|wow)$'i, ),

block images
  '0img' => new_RE( qr'.\.(ai|art|att|bmp|cal|cdr|cdt|cdx|cmf|cmp|dib|drw|emf|eps|fh3|fif|fpx|gem|gif|html|icb|iff|ima|img|jbf|jff|jif|jpeg|jpg|jtf|kdc|kfx|lbm|mac|mic|pbm|pcd|pcs|pct|pcx|pgm|pic|pif|png|pnt|ppm|ps|psd|ras|raw|sct|sdr|sdt|sep|shg|tga|tif|tiff|vda|vst|wil|wmf|wpg|wvl)$'i, ),

