# Changelog

## 1.3.0
Reformat Code for manifests (Code | Reformat Code, Ctrl+Alt+L, also for a selection):
- Indentation by nesting: 2 spaces per level (or the step the file already uses), attributes below a title on its own
  line one level deeper, closing brackets back at the opening line's indentation. Code that indents one level per
  bracket (`create_resources('x', {`) keeps that style.
- `=>` arrows in resource bodies (and the hashes and selectors nested in them) are aligned one space past the longest
  key, which is what puppet-lint's arrow_alignment check expects. Blocks with several entries on one line are left alone.
- Trailing whitespace is removed. Continuation lines keep their position relative to the statement, comments in the
  first column stay there, and strings, heredocs and multi-line comments are never changed. Only whitespace is changed.
- Tested on 974 manifests from 22 lint-clean modules (puppetlabs, voxpupuli, theforeman): apart from trailing
  whitespace, it changed 13 of 65,000 lines.

## 1.2.1
OpenVox 9.5 support: resource type and fact data now also covers OpenVox (the community fork of Puppet), so code written
for OpenVox 9.5 is no longer flagged. The new `package` parameter `environment` is known (completion and docs mark it as
OpenVox-only), and the new `networking.interfaces.<interface>` facts (`description`, `flags`, `groups`, `rdomain`) are completed.

## 1.2.0
Hiera data support (YAML files under `data/` or `hieradata/`; uses the bundled YAML plugin):
- Keys like `ntp::servers` or `profile::web::port` link to the class and the parameter they set by automatic parameter
  lookup: Ctrl+click goes to the parameter, Find Usages on a parameter lists the data files that set it, and renaming a
  parameter renames the keys.
- New inspection: a key for a class in your project that has no such parameter (typo, removed or renamed parameter) is
  flagged, because Puppet silently ignores it. Includes a "did you mean" fix. Keys read with `lookup()`/`hiera()` in your
  code or with `%{alias('...')}` in data are not flagged; classes that aren't in the project are never flagged.
- Completion of `class::parameter` keys at the top level of data files, and of data keys and parameters inside `lookup('...')`.
- `lookup('key')` jumps to the key in all data files (or to the class parameter when no data sets it).

## 1.1.0
EPP template support (`*.epp`):
- Highlighting of template tags and of the Puppet code inside them.
- The code of all tags is parsed as one Puppet program, like Puppet does, so blocks spanning several tags
  (`<% $list.each |$x| { -%> ... <% } -%>`, `if`/`elsif`/`else`, `case`) work.
- Legacy facts (`<%= $::fqdn %>`, `$facts['osfamily']`) are flagged with the one-click Puppet 8 fix, as in manifests.
- Template parameters: go to declaration, find usages, completion.
- Errors for unclosed tags and a parameter tag that is not at the start of the template.
- Comment with block comment in template text inserts `<%# %>`.

Manifest fixes (found in 13 more modules, now tested on ~1,000 manifests and 260 templates from 51 modules):
- `Resource['type'] { $title: ... }` declarations and resource declarations inside arrays (`-> [ foo { 'a': }, bar { 'b': } ]`) parse correctly.
- `file` `target` accepts any link target (was wrongly limited to `notlink`).

## 1.0.1
Parser and inspection fixes found by testing against ~790 manifests from 38 popular Puppet modules:
- `if`/`unless`/`case` used as values no longer cause parse errors.
- Function calls with a trailing hash without braces (`epp('t.epp', key => $v)`) parse correctly.
- Keywords as hash keys (`{ environment => ... }`) are accepted.
- Resource overrides (`File['x'] { notify +> ... }`) and capitalized declarations (`Exec { 'title': ... }`) are parsed and checked.
- Custom types written in Ruby (`lib/puppet/type/*.rb`, e.g. archive, firewall, vcsrepo) are no longer reported as missing defined types.
- Clearer message for `file` `ensure` values that Puppet treats as a symlink target; `ensure => false` is accepted.

## 1.0.0
First release: Puppet/OpenVox manifest highlighting, Puppet 8 legacy-fact inspection with quick fix, completion (types, attributes, values, functions, classes, facts), cross-file navigation, inspections, quick documentation.
