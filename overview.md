# SemiBlock Java

SemiBlock Java is a visual, block-based programming environment for writing Java code. It is built on Blockly 11 and provides a rich toolbox of Java-oriented blocks for classes, methods, control flow, database access, and more. Code is generated in real time as Java source and can be inspected or executed in a host application.

## Key Features

- **Blockly canvas** with the custom `quantr` theme, zelos renderer, 20-pixel snap grid, and trashcan.
- **Custom category UI** (`CustomCategory`) with colored icons (primarily `java.svg`) and dynamic selection shading via `shadeColor` / `rgbToHex`.
- **Live code generation** using a dedicated `javaGenerator`. Every meaningful workspace change triggers `javaGenerator.workspaceToCode` and updates the sidebar.
- **ANTLR-powered visualization**: Generated Java is parsed with a Java 20 grammar (ANTLR4 `Java20Lexer` / `Java20Parser` / `Java20ParserListener`). Class inheritance (`NormalClassDeclaration` + `classExtends`) is extracted and rendered as a Graphviz SVG diagram (via `@viz-js/viz`) in the output pane.
- **Persistence**: Workspace state is automatically saved to `localStorage` under the key `mainWorkspace` (via `serialization.js`). Custom `saveWS` / `loadWS` / `getWS` helpers also exist (binary encoding path in some builds).
- **Exported API** (when built as the `quantr` library):
  - `quantr.clearWorkspace()`
  - `quantr.saveWS()`
  - `quantr.load(json)`
  - `quantr.getWS()`
  - `quantr.getCode()`
- **Build output**: Webpack bundles the editor to `public/blockly-java/build-production` (library target `var` named `quantr`). It can be embedded in larger SemiBlock pages.

## Toolbox Categories

The toolbox is defined in `toolbox.js` (which imports `toolboxJava.js`) and contains these top-level categories:

- **Java** (`java_category`, #a2d2ff) — Core statements and helpers: `newline`, `comment`, `identifier`, `createAttribute`, `createFunction`, `createMainMethod`, `sout`, `forLoop`, `freeCode` (multiline), `tryCatch`.
- **Class** (`class_category`, #a2d2ff) — Object-oriented building blocks: `createClass` (with optional `extends`), `createAttribute`, `createMethod`.
- **Logic** (`logic_category`, #a2d2ff) — Control structures: `if_statement`, `if_else_statement`, `while_loop`, `do_while_loop`, `switch_statement`, `case_statement`.
- **Database** (`database_category`, #ffafcc) — MySQL / JDBC blocks: `mysql_connect`, `mysql_execute_query`, `mysql_close_connection`, `mysql_prepare_statement`, `mysql_execute_update`, `mysql_fetch_results`, `mysql_handle_exception`.
- **Functional** (`fp_category`, #cdb4db) — Currently contains `comment`.
- **Machine Learning** (`ml_category`, #48cae4) — Experimental: `ml_load_model`, `ml_predict`, `ml_train_model`.
- **Generative AI** (`gai_category`, #5b6cf1) and **Web Dev** (`webdev_category`, #e63877) — Placeholder categories (contain `comment`).

All block definitions live in `blocks/javaBlock.js` (registered via `everything` array). Generators are in `generators/java.js` (using a shared `myStatementsToCode` helper for nested statement inputs).

## Special Blocks

- `createClass` / `createMainMethod` / `createFunction` / `createMethod` — produce properly indented Java members and classes.
- `freeCode` — accepts raw multiline Java via `@blockly/field-multilineinput`.
- `sout` — quick `System.out.println(...)`.
- Full MySQL suite — emits JDBC boilerplate (`DriverManager.getConnection`, `Statement` / `PreparedStatement`, `ResultSet`, etc.).
- Logic blocks — simple condition + statement-list style (no expression blocks yet).

## Sidebar Output

The right-hand pane (400 px) shows:
1. Generated Java source (`#generatedCode`).
2. Live inheritance diagram (`#output`) produced by parsing the generated code with ANTLR and feeding class → superclass edges to Graphviz.

## Integration Notes

- The editor is intended to be loaded inside the larger SemiBlock / NewBlock platform.
- Host pages can call the exported `quantr.*` functions and read `getCode()` to obtain the emitted Java text.
- The ANTLR + viz step demonstrates how visual Java models can be statically analyzed for documentation or diagramming purposes.

See the [Getting Started](getting-started.md) page for a walkthrough of building a small Java program.