Release 1.5.1
=============

**Fixes**

* Fixed a bug in `InsertChildAt`.

Release 1.5.0
=============

**Changes**

* Added `Element` function `CreateChild`, which calls a continuation function
  after creating and adding a child element.

**Fixes**

* Removed a potential conflict between two `ReadSettings` values. When
  `AttrSingleQuote` is true, `CanonicalAttrVal` is forced to be false.

Release 1.4.1
=============

**Changes**

* Minimal go version updated to 1.21.
* Default-initialized CharsetReader causes same result as NewDocument().
* When reading an XML document, attributes are parsed more efficiently.

Release v1.4.0
==============

**New Features**

* Add `AutoClose` option to `ReadSettings`.
* Add `ValidateInput` to `ReadSettings`.
* Add `NotNil` function to `Element`.
* Add `NextSibling` and `PrevSibling` functions to `Element`.

Release v1.3.0
==============

**New Features**

* Add support for double-quotes in filter path queries.
* Add `PreserveDuplicateAttrs` to `ReadSettings`.
* Add `ReindexChildren` to `Element`.

Release v1.2.0
==============

**New Features**

* Add the ability to write XML fragments using Token WriteTo functions.
* Add the ability to re-indent an XML element as though it were the root of
  the document.
* Add a ReadSettings option to preserve CDATA blocks when reading and XML
  document.

Release v1.1.4
==============

**New Features**

* Add the ability to preserve whitespace in leaf elements during indent.
* Add the ability to suppress a document-trailing newline during indent.
* Add choice of XML attribute quoting style (single-quote or double-quote).

**Removed Features**

* Removed the CDATA preservation change introduced in v1.1.3. It was
  implemented in a way that broke the ability to process XML documents
  encoded using non-UTF8 character sets.

Release v1.1.3
==============

* XML reads now preserve CDATA sections instead of converting them to
  standard character data.

Release v1.1.2
==============

* Fixed a path parsing bug.
* The `Element.Text` function now handles comments embedded between
  character data spans.

Release v1.1.1
==============

* Updated go version in `go.mod` to 1.20

Release v1.1.0
==============

**New Features**

* New attribute helpers.
  * Added the `Element.SortAttrs` method, which lexicographically sorts an
    element's attributes by key.
* New `ReadSettings` properties.
  * Added `Entity` for the support of custom entity maps.
* New `WriteSettings` properties.
  * Added `UseCRLF` to allow the output of CR-LF newlines instead of the
    default LF newlines. This is useful on Windows systems.
* Additional support for text and CDATA sections.
  * The `Element.Text` method now returns the concatenation of all consecutive
    character data tokens immediately following an element's opening tag.
  * Added `Element.SetCData` to replace the character data immediately
    following an element's opening tag with a CDATA section.
  * Added `Element.CreateCData` to create and add a CDATA section child
    `CharData` token to an element.
  * Added `Element.CreateText` to create and add a child text `CharData` token
    to an element.
  * Added `NewCData` to create a parentless CDATA section `CharData` token.
  * Added `NewText` to create a parentless text `CharData`
    token.
  * Added `CharData.IsCData` to detect if the token contains a CDATA section.
  * Added `CharData.IsWhitespace` to detect if the token contains whitespace
    inserted by one of the document Indent functions.
  * Modified `Element.SetText` so that it replaces a run of consecutive
    character data tokens following the element's opening tag (instead of just
    the first one).
* New "tail text" support.
  * Added the `Element.Tail` method, which returns the text immediately
    following an element's closing tag.
  * Added the `Element.SetTail` method, which modifies the text immediately
    following an element's closing tag.
* New element child insertion and removal methods.
  * Added the `Element.InsertChildAt` method, which inserts a new child token
    before the specified child token index.
  * Added the `Element.RemoveChildAt` method, which removes the child token at
    the specified child token index.
* New element and attribute queries.
  * Added the `Element.Index` method, which returns the element's index within
    its parent element's child token list.
  * Added the `Element.NamespaceURI` method to return the namespace URI
    associated with an element.
  * Added the `Attr.NamespaceURI` method to return the namespace URI
    associated with an element.
  * Added the `Attr.Element` method to return the element that an attribute
    belongs to.
* New Path filter functions.
  * Added `[local-name()='val']` to keep elements whose unprefixed tag matches
    the desired value.
  * Added `[name()='val']` to keep elements whose full tag matches the desired
    value.
  * Added `[namespace-prefix()='val']` to keep elements whose namespace prefix
    matches the desired value.
  * Added `[namespace-uri()='val']` to keep elements whose namespace URI
    matches the desired value.

**Bug Fixes**

* A default XML `CharSetReader` is now used to prevent failed parsing of XML
  documents using certain encodings.
  ([Issue](https://github.com/beevik/etree/issues/53)).
* All characters are now properly escaped according to XML parsing rules.
  ([Issue](https://github.com/beevik/etree/issues/55)).
* The `Document.Indent` and `Document.IndentTabs` functions no longer insert
  empty string `CharData` tokens.

**Deprecated**

* `Element`
    * The `InsertChild` method is deprecated. Use `InsertChildAt` instead.
    * The `CreateCharData` method is deprecated. Use `CreateText` instead.
* `CharData`
    * The `NewCharData` method is deprecated. Use `NewText` instead.


Release v1.0.1
==============

**Changes**

* Added support for absolute etree Path queries. An absolute path begins with
  `/` or `//` and begins its search from the element's document root.
* Added [`GetPath`](https://godoc.org/github.com/beevik/etree#Element.GetPath)
  and [`GetRelativePath`](https://godoc.org/github.com/beevik/etree#Element.GetRelativePath)
  functions to the [`Element`](https://godoc.org/github.com/beevik/etree#Element)
  type.

**Breaking changes**

* A path starting with `//` is now interpreted as an absolute path.
  Previously, it was interpreted as a relative path starting from the element
  whose
  [`FindElement`](https://godoc.org/github.com/beevik/etree#Element.FindElement)
  method was called.  To remain compatible with this release, all paths
  prefixed with `//` should be prefixed with `.//` when called from any
  element other than the document's root.
* [**edit 2/1/2019**]: Minor releases should not contain breaking changes.
  Even though this breaking change was very minor, it was a mistake to include
  it in this minor release. In the future, all breaking changes will be
  limited to major releases (e.g., version 2.0.0).

Release v1.0.0
==============

Initial release.
