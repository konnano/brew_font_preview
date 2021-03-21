use strict;
use Exporter;
use vars qw(@ISA @EXPORT @EXPORT_OK $VERSION);

@ISA = qw/ Exporter /;
@EXPORT = qw(cwd getcwd fastcwd fastgetcwd);
push @EXPORT, qw(getdcwd) if $^O eq 'MSWin32';
@EXPORT_OK = qw(chdir abs_path fast_abs_path realpath fast_realpath);

sub _perl_abs_path{
    my $start = @_ ? shift : '.';
    my($dotdots, $cwd, @pst, @cst, $dir, @tst);
    @cst = stat( $start );
    $cwd = '';
    $dotdots = $start;
    do{
	$dotdots .= '/..';
	@pst = @cst; #print"pst : $_\n" for(@pst);

	opendir(PARENT, $dotdots); #print"$dotdots\n";
	@cst = stat($dotdots); #print"cst : $_\n" for(@cst);
	  if ($pst[0] == $cst[0] && $pst[1] == $cst[1]){ #print"$dir\n";
	    $dir = undef;
	  }else{
	    do{ $dir = readdir(PARENT); print"$dir\n";
		$tst[0] = $pst[0]+1 unless (@tst = lstat("$dotdots/$dir"))
	    }while ($dir eq '.' || $dir eq '..' || $tst[0] != $pst[0] || $tst[1] != $pst[1]);
	  }
	$cwd = (defined $dir ? "$dir" : "" ) . "/$cwd" ; print"koko $cwd\n";
	closedir(PARENT);
    }while (defined $dir);
    chop($cwd) unless $cwd eq '/';
    $cwd;
}

*fast_realpath = \&fast_abs_path;
*_NT_cwd = defined &Win32::GetCwd ? \&_win32_cwd : \&_win32_cwd_simple;
*abs_path = \&_perl_abs_path unless defined &abs_path;
*getcwd = \&_perl_getcwd unless defined &getcwd;
*realpath = \&abs_path;

1;
