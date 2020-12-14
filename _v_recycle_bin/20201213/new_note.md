<html>
<head>
  <!--

	+-------------------------------------------------------------+
	|                                                             |
	|             Nullboard, a minimalist kanban board            |
	|                    https://nullboard.io                     |
	|                                                             |
	+-------------------------------------------------------------+

	LICENSE

	The 2-clause BSD license with the Commons Clause condition.

	IN SHORT

	You can use, change and distribute the software free of charge
	provided that you do not sell it or make money off it in
	certain less direct ways as specified below. When distributed,
	the software must include a complete copy of this license.

	IN FULL

	The Software is provided to you by the Licensor under the
	License, as defined below, subject to the following condition.

	Without limiting other conditions in the License, the grant of
	rights under the License will not include, and the License does
	not grant to you, the right to Sell the Software.

	For purposes of the foregoing, "Sell" means practicing any or
	all of the rights granted to you under the License to provide
	to third parties, for a fee or other consideration (including
	without limitation fees for hosting or consulting/ support
	services related to the Software), a product or service whose
	value derives, entirely or substantially, from the functionality
	of the Software. Any license notice or attribution required
	by the License must also include this Commons Clause License
	Condition notice.

	Software:  Nullboard
	License:   The 2-clause BSD License
	Licensor:  Alexander Pankratov / swapped.ch

  -->
  <meta charset="utf-8">
  <title>Nullboard</title>
  <style>

	/***/
	@font-face {
		font-family: 'Nullboard';
		font-weight: 400;
		font-style: normal;
		src: url('extras/Barlow-Regular.woff') format('woff');
	}

	@font-face {
		font-family: 'Nullboard';
		font-weight: 500;
		font-style: normal;
		src: url('extras/Barlow-Medium.woff') format('woff');
	}

	/***/
	html, body, h1, textarea, input {
		padding: 0;
		margin: 0;
	}

	body, input, textarea {
		font-family: Nullboard, sans-serif;
		font-size: 13px;
	}

	body {
		background: #fff;
		background: #f8f9fb;
	}

	body.dragging {
		user-select: none;
		-webkit-user-select: none;
		-ms-user-select: none;
		-webkit-touch-callout: none;
		-o-user-select: none;
		-moz-user-select: none;
	}

	body.ding {
		padding-top: 2px;
	}

	a {
		text-decoration: none;
		transition: color 200ms;
	}

	a, a:active, a:focus, textarea, input {
		outline: none;
	}

	tt {
		display: none;
	}

	/***/
	.clearfix:after,
	.board:after,
	.lists:after,
	.notes:after,
	.head:after,
	.menu:after {
		content: "";
		display: table;
		clear: both;
	}

	/***/
	.board {
		min-width: 250px;

		width: -moz-max-content;    /* firefox */
		width: -webkit-max-content; /* chrome  */
		width: intrinsic;           /* safari  */

		margin: 0 auto;
		padding: 20px;

		user-select: none;
	}

	body.crowded .board {
		margin-top: 28px;
	}

	.board u {
		text-decoration: none;
	}

	.board u:before {
		content: '\00D7';
		position: relative;
		top: 2px;
		font-size: 17px;
		line-height: 10px;
		font-weight: 400;
	}

	/***/
	.board .head {
		padding: 5px 0;
		margin: 0 5px;
		position: relative;
	}

	.board .head .text,
	.board .head .edit {
		font-weight: 500;
		font-size: 13.5px;
		line-height: 20px;
		padding: 0 5px 2px;
		border: none;
	}

	.board .head .text {
		min-height: 20px;
		white-space: pre;
		overflow: hidden;
	}

	.board .head .edit {
		display: none;
	}

	.board .head .edit::placeholder {
		font-weight: 400;
		font-size: 10px;
		line-height: 22px;
		text-transform: uppercase;
		color: #1489db;
		opacity: 0.8;
	}

	.board .head.editing .text {
		display: none;
	}

	.board .head.editing .edit {
		display: block;
		outline: 1px solid #8eaedd;
	}

	.board .menu {
		position: absolute;
		top: 0;
		right: 0;
		height: 20px;
		padding: 5px 6px 7px 30px;
		background: linear-gradient(to right, #EAEDF000, #EAEDF0 10px);
		font-size: 11px;
		line-height: 20px;
	}

	.board .menu a,
	.board .ops  a {
		color: #000000A0;
		transition: color 200ms;
	}

	.board .menu a {
		padding-left: 10px;
	}

	.board .menu a:hover,
	.board .ops  a:hover {
		color: #000;
	}

	.board .menu a.warn:hover,
	.board .ops  a.warn:hover {
		color: #c40;
	}

	.board .menu .undo-board,
	.board .menu .redo-board {
		display: none;
	}

	.board .head.editing .menu {
		display: none;
	}

	.board > .head {
		background: #EAEDF0;
		padding: 5px;
		margin: 0 0 10px;
		border-radius: 2px;
		position: relative;
	}

	.board > .head .menu {
		margin-right: 5px;
	}

	/***/
	.board .lists-scroller {
		height: auto;
		margin: -1px 0 10px;
		overflow-x: auto;
		overflow-y: hidden;
		display: none;
	}

	.lists-scroller div {
		height: 1px;
	}

	.board .lists {
		white-space: nowrap;
		overflow: auto;
		scrollbar-width: none;
	}

	.board .list {
		display: inline-block;
		vertical-align: top;
		width: 250px;
		margin: 0 5px 10px;
		background: linear-gradient(#EAEDF0 30px, #DDE1E5 90px);
		border-radius: 2px;
	}

	.board .list::-webkit-scrollbar {
		display: none;
	}

	.board .list:first-child {
		margin-left: 0;
	}

	.board .list:last-child {
		margin-right: 0;
	}

	.board .list .notes {
		padding: 0 5px;
	}

	/***/
	.board .head .menu .teaser {
		position: absolute;
		right: 3px;
		top: 5px;
		padding: 0 3px;
	}

	.board .head .menu .bulk {
		display: none;
		opacity: 0;
		z-index: 1;
	}

	.board .head .menu:hover .bulk {
		display: block;
		opacity: 1;
	}

	.board .head .menu:hover .teaser {
		display: none;
	}

	/***/
	.board .list .menu .mov-list-r.half {
		padding-left: 0;
	}

	.board .list .menu .full {
		display: none;
	}

	.board .list:first-child .menu .half,
	.board .list:last-child  .menu .half {
		display: none;
	}

	.board .list:first-child .menu .mov-list-r.full,
	.board .list:last-child  .menu .mov-list-l.full {
		display: inline-block;
	}

	.board .list:first-child:last-child .menu .half,
	.board .list:first-child:last-child .menu .full {
		display: none;
	}

	/***/
	.board .note {
		background: #fff;
		margin-top: 5px;
		box-shadow: 0 1px 2px #bbb, 0 0 1px #ddd;
		position: relative;
	}

	.board .note.dragging,
	.board .note.dragging.raw {
		background: #CED4DA;
		box-shadow: 0 +1px 0 #00000010 inset,
		            0 -1px 0 #00000010 inset,
		            +1px 0 0 #00000010 inset,
		            -1px 0 0 #00000010 inset;
	}

	.board .note.dragging * {
		opacity: 0 !important;
	}

	/***/
	.board .note:last-child {
		margin-bottom: 5px;
	}

	.board .note .text,
	.board .note .edit {
		padding: 5px 10px;
		margin-right: 15px;
		font-size: 11px;
	}

	.board .note .text {
		min-height: 13px;
		white-space: pre-wrap;
		overflow-wrap: anywhere;
	}

	/***/
	.board .head .text a,
	.board .note .text a {
		color: inherit;
		cursor: default;
		transition: none;
	}

	@keyframes whoomp {
		0%   { color: inherit; }
		30%  { color: #888;    }
		100% { color: inherit; }
	}

	.board .head .text a:hover,
	.board .note .text a:hover {
		animation-name: whoomp;
		animation-duration: 700ms;
	}

	.reveal .board .head .text a,
	.reveal .board .note .text a {
		color: #1489db;
		cursor: pointer;
	}

	.reveal .board .head .text a:hover,
	.reveal .board .note .text a:hover {
		animation-name: none;
	}

	/***/
	.board .note .edit {
		display: none;
		border: none;
	}

	.board .note.editing {
		box-shadow: none;
		outline: 1px solid #8eaedd;
	}

	.board .note.editing .text {
		display: none;
	}

	.board .note.editing .edit {
		display: block;
		resize: none;
	}

	/***/
	.board .note .ops {
		position: absolute;
		top: 0;
		right: 0;
		opacity: 0;
		transition: opacity 400ms;
		cursor: default;
		font-size: 9px;
	}

	.board .note.editing .ops {
		display: none;
	}

	.board .note:hover .ops {
		opacity: 1;
	}

	.board .note .ops .teaser {
		display: block;
		margin-top: 2px;
		margin-right: 1px;
		padding-right: 3px;
	}

	.board .note .ops .teaser:before {
		content: '\2261';
	}

	.board .note .ops .bulk {
		display: none;
		background: #fff;
		border-left: none;
		padding: 0 0 1px 5px;
		font-weight: 500;
		border-left: 1px solid #ddd;
		border-bottom: 1px solid #ddd;
	}

	.board .note .ops .bulk a {
		padding-right: 4px;
	}

	.board .note .ops:hover .bulk {
		display: block;
	}

	.board .note .ops:hover .teaser {
		display: none;
	}

	/***/
	.board .note.raw {
		background: transparent;
		box-shadow: none;
		font-weight: 500;
	}

	.board .note.raw.editing {
		background: #fff;
	}

	.board .note.raw .text {
	}

	.board .note.collapsed {
		height: 23px;
	}

	.board .note.collapsed .text {
		height: 13px;
		overflow: hidden;
		line-height: 22px;
		padding-top: 0;
	}

	.board .note.collapsed .ops {
		opacity: 1;
	}

	.board .note.collapsed .ops .teaser {
		padding: 1px 3px 0 1px;
	}

	.board .note.collapsed .ops .teaser:before {
		content: '_';
		top: 1px;
	}

	.board .note.collapsed:hover .ops .teaser:before {
		content: '\2261';
	}

	/***/
	.dragster {
		z-index: 2;
		position: absolute;
		opacity: 0;
		box-shadow: 1px 2px 3px #00000050;
		background: #fff;
		white-space: pre-wrap;
		cursor: move;

		padding: 5px 25px 5px 10px;
		font-size: 11px;

		box-shadow: 0 +1px 0 #ACB4BC inset,
		            0 -1px 0 #ACB4BC inset,
		            +1px 0 0 #ACB4BC inset,
		            -1px 0 0 #ACB4BC inset,
		            0px 1px 3px #00000030;
	}

	.dragster a {
		color: #000;
	}

	.dragster.collapsed {
		overflow: hidden;
		line-height: 22px;
		padding-top: 0;
	}

	/***/
	.logo {
		position: absolute;
		top: 9px;
		left: 9px;
		font-size: 11px;
		line-height: 19px;
		padding: 6px 12px;
		opacity: 0.6;
		z-index: 3;
		background: #f8f9fb;
	}

	body.crowded .logo:hover {
		background: #fff;
		box-shadow: 0 1px 2px rgba(0,0,0,0.2);
	}

	.logo:hover {
		opacity: 1.0;
	}

	.logo .bulk {
		overflow: hidden;
		height: 0;
		opacity: 0;
		transition: opacity 400ms;
	}

	.logo:hover .bulk {
		height: auto;
		opacity: 1;
	}

	.logo a {
		color: #000;
		display: block;
	}

	.logo a:hover {
		color: #1489db;
	}

	.logo .bulk a:before {
		display: inline-block;
		content: '-';
		width: 11px;
	}

	/***/
	.config {
		position: absolute;
		top: 10px;
		right: 21px;
		font-size: 11px;
		line-height: 19px;
		z-index: 3;
		background: #f8f9fb;
	}

	body.crowded .config:hover {
		background: #fff;
		box-shadow: 0 1px 2px rgba(0,0,0,0.2);
	}

	.config a {
		display: block;
		color: #000;
		transition: color 200ms;
	}

	.config a:hover {
		color: #1489db;
	}

	.config .teaser {
		padding: 5px;
		color: #999;
	}

	.config .bulk {
		margin-right: 20px;
		padding: 5px 0 10px 22px;
		transition: opacity 400ms;
	}

	.config .bulk div {
		display: none;
		padding: 6px 2px;
		margin: 6px -2px;
		border-top: 1px solid #00000028;
		border-bottom: 1px solid #00000028;
	}

	.config .bulk div a {
		display: block;
		margin-right: 10px;
	}

	.config .bulk div a.active {
		color: #1489db;
	}

	.config .bulk div a.active:before {
		content: '\2022 ';
		display: inline-block;
		width: 13px;
		margin-left: -13px;
	}

	.config .bulk div a.active:hover {
		color: #000;
	}

	.config .bulk a i,
	.config .bulk a b {
		font-style: normal;
		font-weight: inherit;
	}

	.config .bulk a i {
		display: none;
	}

	.config .bulk input.imp-board-select {
		position: absolute;
		width: 1px;
		height: 1px;
		visibility: hidden;
	}

	.config .bulk a.switch-theme {
		padding-top: 6px;
		margin-top: 6px;
		border-top: 1px solid #00000028;
	}

	/***/
	.config .bulk {
		display: none;
		opacity: 0;
	}

	.config:hover .teaser {
		display: none;
	}

	.config:hover .bulk {
		display: block;
		opacity: 1;
	}

	/***/
	.overlay {
		position: fixed;
		z-index: 10;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.9);
		display: none;
		color: #000;
	}

	/***/
	.overlay .license {
		font-size: 12px;
		line-height: 16px;
		white-space: pre-wrap;
		width: 384px;
		height: auto;
		max-height: 600px;
		padding: 20px 25px 22px;
		overflow-y: auto;
		background: #fff;
		position: absolute;
		top: 100px;
		left: 50%;
		margin-left: -192px;
	}

	.overlay .license a {
		color: #1489db;
	}

	.overlay .license span {
		display: inline-block;
		padding-right: 8px;
	}

	/***/
	.overlay .about {
		font-size: 12px;
		line-height: 16px;
		text-align: center;
		width: 240px;
		height: auto;
		padding: 50px 25px;
		background: #fff;
		position: absolute;
		top: 100px;
		left: 50%;
		margin-left: -145px;
	}

	.overlay .about h1 {
		font-size: 15px;
		font-weight: 500;
		margin-bottom: 10px;
	}

	.overlay .about a {
		display: block;
		color: #1489db;
	}

	.overlay .about div {
		position: absolute;
		bottom: -30px;
		width: 240px;
		color: #888;
	}

	.overlay .about div span {
		display: inline-block;
		text-align: left;
		padding-right: 10px;
	}

	/***/

	@media print {
		.logo, .config,
		.board .head .teaser,
		.list .head .teaser {
			visibility: hidden;
			display: none;
		}

		.board .note {
			box-shadow: none;
			outline: 1px solid #ccc;
		}

		.board .note.raw {
			outline: none;
		}
	}

	/*
	 *	Dark mode
	 */
	body.dark {
		background-color: #15171A;
		color: #d6d6d6;
	}

	.dark .board .head {
		background: #202224;
	}

	/***/
	.dark .board .menu {
		background: linear-gradient(to right, #20222400, #202224 10px);
	}

	.dark .board .menu a {
		color: #aaa;
	}

	.dark .board .menu a:hover {
		color: #fc4;
	}

	.dark .board .menu a.warn:hover,
	.dark .board .ops  a.warn:hover {
		color: #fc5555 !important;
	}

	.dark .board .head {
		color: #eee;
	}

	.dark .board .head .edit::placeholder {
		color: #bf9d21;
	}

	/***/
	.dark .board .list {
		background: #202224;
	}

	.dark .board .note {
		background: #2B2C2F;
		background: linear-gradient(#2B2C2F, #27282B);
		box-shadow: 0 1px 3px #0005;
		text-shadow: 0 0 4px #0008;
	}

	.dark.reveal .board .head .text a,
	.dark.reveal .board .note .text a {
		color: #fc0;
	}

	@keyframes whoomp-dark {
		0%   { color: inherit; }
		30%  { color: #fff;    }
		100% { color: inherit; }
	}

	.dark .board .head .text a:hover,
	.dark .board .note .text a:hover {
		animation-name: whoomp-dark;
	}


	.dark .board .note.raw {
		background: transparent;
		box-shadow: none;
	}

	.dark .board .note.raw .text {
		color: #eee;
		text-shadow: 0 0 4px #0008;
	}

	.dark .board .note .ops .teaser {
		color: #ccc;
	}

	.dark .board .note .ops .bulk {
		background: #202224;
		border-left: 1px solid #111;
		border-bottom: 1px solid #111;
	}

	.dark .board .note.raw .ops .bulk {
		border: none;
	}

	.dark .board .note .ops .bulk a {
		color: #ccc;
	}

	.dark .board .note .ops .bulk a:hover {
		color: #fc2;
	}

	.dark .board .note.dragging,
	.dark .board .note.dragging.raw {
		background: #15171A;
		box-shadow: 0 +1px 0 #000 inset,
		            0 -1px 0 #000 inset,
		            +1px 0 0 #000 inset,
		            -1px 0 0 #000 inset;
	}

	.dark .dragster {
		box-shadow: 0px 1px 4px #000000;
		background: #2e3032;
	}

	.dark .dragster a {
		color: #d6d6d6;
	}


	/***/
	.dark textarea,
	.dark input {
		background: #111315;
		color: #eee;
	}

	.dark .board .head.editing .edit {
		outline: 1px solid #bc9908;
	}

	.dark .board .note.editing {
		background: #111315;
		outline: 1px solid #bc9908;
	}

	/***/
	.dark .config, .dark.crowded .config:hover {
		background-color: #15171A;
	}

	.dark .config a {
		color: #ddd;
	}

	.dark .config a:hover {
		color: #fc2;
	}

	.dark .config .bulk div {
		border-top: 1px solid #fff2;
		border-bottom: 1px solid #fff2;
	}

	.dark .config .bulk div a.active {
		color: #fc2;
	}

	.dark .config .bulk a.switch-theme {
		border-top: 1px solid #fff2;
	}

	.dark .config a.switch-theme i {
		display: inline;
	}

	.dark .config a.switch-theme b {
		display: none;
	}

	/***/
	.dark .logo a {
		color: #ccc;
	}

	.dark .logo, .dark.crowded .logo:hover {
		background: #15171A;
	}

	.dark .logo a:hover {
		color: #fc2;
	}

	.dark .logo > a:hover {
		color: #fff;
	}

	/*
	 *	Larger font
	 */
	body.z1,
	body.z1 input,
	body.z1 textarea {
		font-size: 15px;
	}

	.z1 .board {
		min-width: 290px;
	}

	.z1 .board u:before {
		font-size: 19px;
		line-height: 12px;
	}

	.z1 .board .head .text {
		min-height: 22px;
	}

	.z1 .board .head .text,
	.z1 .board .head .edit {
		font-size: 15px;
		line-height: 22px;
	}

	.z1 .board .head .edit::placeholder {
		font-size: 10px;
		line-height: 23px;
	}

	.z1 .board .menu {
		height: 22px;
		font-size: 13px;
		line-height: 22px;
	}

	.z1 .board .list {
		width: 290px;
	}

	.z1 .board .note .text,
	.z1 .board .note .edit,
	.z1 .dragster {
		font-size: 13px;
	}

	.z1 .board .note .text {
		min-height: 16px;
	}

	.z1 .board .note.collapsed {
		height: 26px;
	}

	.z1 .board .note.collapsed .text {
		height: 17px;
		line-height: 25px;
	}

	.z1 .dragster.collapsed {
		line-height: 24px;
	}

	.z1 .board .note .ops {
		font-size: 10px;
	}

	/***/
	.z1 .logo {
		font-size: 13px;
		line-height: 21px;
	}

	.z1 .logo .bulk a:before {
		width: 13px;
	}

	/***/
	.z1 .config {
		font-size: 13px;
		line-height: 21px;
	}

	.z1 .config .bulk div a.active:before {
		width: 15px;
	}

	.z1 .config a.switch-fsize i {
		display: inline;
	}

	.z1 .config a.switch-fsize b {
		display: none;
	}

	/***/
	.z1 .overlay .license {
		font-size: 14px;
		line-height: 18px;
		width: 443px;
	}

	.z1 .overlay .about {
		font-size: 12px;
		line-height: 18px;
		width: 277px;
	}

	.z1 .overlay .about h1 {
		font-size: 17px;
		width: 277px;
	}

</style>

</head>
<body>
	<div class=logo>
		<a href=https://nullboard.io>Nullboard</a>
		<div class=bulk>
			<a href=# class=view-about>About</a>
			<a href=# class=view-license>License</a>
			<a href=https://nullboard.io/github  target=_blank>Github</a>
			<a href=https://nullboard.io/twitter target=_blank>Twitter</a>
		</div>
	</div>

	<div class=config>
		<a href=# class=teaser>&equiv;</a>
		<div class=bulk>
			<a href=# class=add-board>Add new board...</a>
			<div class=boards>
				<!-- here'll be boards -->
			</div>
			<a href=# class=imp-board>Import board...</a>
				<input class=imp-board-select type="file" accept=".nbx">
			<a href=# class=exp-board>Export board...</a>
			<a href="#" class="switch-theme">Use <i>light</i><b>dark</b> theme</a>
			<a href="#" class="switch-fsize">Use <i>smaller</i><b>larger</b> font</a>
		</div>
	</div>
	</div>

	<div class=wrap>
	</div>

	<div class=overlay>
	</div>

	<tt>
		<!-- templates -->
		<div class=board>
			<div class=head>
				<div class=text></div>
				<input class=edit spellcheck=false placeholder='Name of the board'>
				<div class=menu>
					<a href=# class=teaser>&equiv;</a>
					<div class=bulk>
						<a href=# class='del-board warn'><u></u> Board</a>
						<a href=# class='undo-board'>Undo</a>
						<a href=# class='redo-board'>Redo</a>
						<a href=# class='add-list'>+ List</a>
					</div>
				</div>
			</div>
			<div class=lists-scroller><div></div></div>
			<div class=lists>
			</div>
		</div>

		<div class=list>
			<div class=head>
				<div class=text></div>
				<input class=edit spellcheck=false placeholder='Name of the list'>
				<div class=menu>
					<a href=# class=teaser>&equiv;</a>
					<div class=bulk>
						<a href=# class='del-list warn'><u></u> List</a>
						<a href=# class='mov-list-l full'>&lt; Move</a>
						<a href=# class='mov-list-l half'>&lt; Mo</a><a href=# class='mov-list-r half'>ve &gt;</a>
						<a href=# class='mov-list-r full'>Move &gt;</a>
						<a href=# class='add-note'>+ Note</a>
					</div>
				</div>
			</div>
			<div class=notes>
			</div>
		</div>

		<div class=note>
			<div class=ops>
				<a href=# class=teaser></a>
				<div class=bulk>
					<a href=# class='del-note warn'><u></u></a>
					<a href=# class='raw-note'>R</a>
					<a href=# class='collapse'>_</a>
				</div>
			</div>
			<div class=text>
			</div>
			<textarea class=edit spellcheck=false></textarea>
		</div>

		<a href=# class=load-board></a>

		<textarea class=encoder></textarea>

		<div class=about>
			<h1>Nullboard</h1>
			Minimalist locally-stored kanban board
			<a href=https://nullboard.io target=_blank>https://nullboard.io</a>
			<div></div>
		</div>

		<div class=license>
		</div>
	</tt>

</body>

<script src="extras/jquery-1.8.3.min.js"></script>
<script>window.jQuery || document.write('<script src="https://code.jquery.com/jquery-1.8.3.min.js">\x3C/script>')</script>
<script type="text/javascript">

	//
	var nb_codeVersion = 20200429;
	var nb_dataVersion = 20190412;

	//
	if (typeof(localStorage) === "undefined")
	{
		alert('Hmmm... no "localStorage" support');
	}

	/*
	 *	poor man's error handling -- $fixme
	 */
	window.onerror = function(message, file, line, col, e){
		var cb1;
		alert("Error occurred: " + e.message);
		return false;
	};

	window.addEventListener("error", function(e) {
		var cb2;
		alert("Error occurred: " + e.error.message);
		return false;
	})

	/*
	 *
	 */
	function Note(text)
	{
		this.text = text;
		this.raw  = false;
		this.min  = false;
	}

	function List(title)
	{
		this.title = title;
		this.notes = [ ];

		this.addNote = function(text)
		{
			var x = new Note(text);
			this.notes.push(x);
			return x;
		}
	}

	function Board(title)
	{
		this.format   = nb_dataVersion;
		this.id       = +new Date();
		this.revision = 1;
		this.title    = title;
		this.lists    = [ ];

		this.addList = function(title)
		{
			var x = new List(title);
			this.lists.push(x);
			return x;
		}
	}

	//
	function htmlEncode(raw)
	{
		return $('tt .encoder').text(raw).html();
	}

	function htmlDecode(enc)
	{
		return $('tt .encoder').html(enc).text();
	}

	//
	function setText($note, text)
	{
		$note.attr('_text', text);

		text = htmlEncode(text);
                hmmm = /\b(https?:\/\/[^\s]+)/mg;

		text = text.replace(hmmm, function(url){
			return '<a href="' + url + '" target=_blank>' + url + '</a>';
		});

		$note.html(text);
	}

	function getText($note)
	{
		return $note.attr('_text');
	}

	//
	function updatePageTitle()
	{
		if (! document.board)
		{
			document.title = 'Nullboard';
			return;
		}

		var $text = $('.wrap .board > .head .text');
		var title = getText( $text );

		document.board.title = title;
		document.title = 'NB - ' + (title || '(unnamed board)');
	}

	function updateUndoRedo()
	{
		var $undo = $('.board .menu .undo-board');
		var $redo = $('.board .menu .redo-board');

		var undo = false;
		var redo = false;

		if (document.board)
		{
			var history = document.board.history;
			var rev = document.board.revision;

			undo = (rev != history[history.length-1]);
			redo = (rev != history[0]);
		}

		if (undo) $undo.show(); else $undo.hide();
		if (redo) $redo.show(); else $redo.hide();
	}

	//
	function showBoard(quick)
	{
		var board = document.board;

		var $wrap = $('.wrap');
		var $bdiv = $('tt .board');
		var $ldiv = $('tt .list');
		var $ndiv = $('tt .note');

		var $b = $bdiv.clone();
		var $b_lists = $b.find('.lists');

		$b[0].board_id = board.id;
		setText( $b.find('.head .text'), board.title );

		board.lists.forEach(function(list){

			var $l = $ldiv.clone();
			var $l_notes = $l.find('.notes');

			setText( $l.find('.head .text'), list.title );

			list.notes.forEach(function(n){
				var $n = $ndiv.clone();
				setText( $n.find('.text'), n.text );
				if (n.raw) $n.addClass('raw');
				if (n.min) $n.addClass('collapsed');
				$l_notes.append($n);
			});

			$b_lists.append($l);
		});

		if (quick)
			$wrap.html('').append($b);
		else
			$wrap.html('')
			  .css({ opacity: 0 })
			  .append($b)
			  .animate({ opacity: 1 });

		updatePageTitle();
		updateUndoRedo();
		updateBoardIndex();
		setupListScrolling();
	}

	//
	function saveBoard()
	{
		var $board = $('.wrap .board');
		var board = new Board( getText($board.find('> .head .text')) );

		$board.find('.list').each(function(){
			var $list = $(this);
			var l = board.addList( getText($list.find('.head .text')) );

			$list.find('.note').each(function(){
				var $note = $(this)
				var n = l.addNote( getText($note.find('.text')) );
				n.raw = $note.hasClass('raw');
				n.min = $note.hasClass('collapsed');
			});
		});

		//
		var rev_new = document.board.history[0] + 1;
		var rev_old = document.board.revision;

		document.board.revision = rev_new;

		board.id       = document.board.id;
		board.revision = document.board.revision;

		var blob_id = board.id + '.' + board.revision;

		localStorage.setItem('nullboard.board.' + blob_id, JSON.stringify(board));
		localStorage.setItem('nullboard.board.' + board.id, board.revision);
		console.log('Saved nullboard.board.' + blob_id + ' of ' + board.title);

		//
		trimBoardHistory(rev_old, rev_new, 50);

		updateUndoRedo();
		updateBoardIndex();
	}

	function parseBoard(blob)
	{
		var board;

		try
		{
			board = JSON.parse(blob);
		}
		catch(x)
		{
			console.log('Malformed JSON');
			return false;
		}

		if (typeof board.format === 'undefined')
		{
			console.log('Board.format is missing');
			return false;
		}

		if (board.format != nb_dataVersion)
		{
			console.log('Board.format is wrong', board.format, nb_dataVersion);
			return false;
		}

		if (! board.revision)
		{
			console.log("Board.revision is missing");
			return false;
		}

		return $.extend(new Board, board);
	}

	function loadBoard(board_id)
	{
		var revision;
		var blob;
		var board;

		revision = localStorage.getItem('nullboard.board.' + board_id);
		if (! revision)
			return false;

		blob = localStorage.getItem('nullboard.board.' + board_id + '.' + revision);
		if (! blob)
			return false;

		board = parseBoard(blob);
		if (! board)
		{
			alert('Whoops. Error parsing board data.');
			console.log('Whoops, there it is:', blob);
			return false;
		}

		if (board.id != board_id || board.revision != revision)
		{
			alert('Whoops. Malformed board.');
			console.log('Whoops, there it is:', board.id, board_id, board.revision, revision);
			return false;
		}

		board.history = loadBoardHistory(board.id);

		return board;
	}

	//
	function loadBoardHistory(board_id)
	{
		var re = new RegExp('^nullboard\.board\.' + board_id + '\.(\\d+)$');
		var r = [];

		for (var i=0; i<localStorage.length; i++)
		{
			var m = localStorage.key(i).match(re);
			if (m) r.push( parseInt(m[1]) );
		}

		r.sort(function(a,b){ return b-a; });
		return r;
	}

	function trimBoardHistory(rev_old, rev_new, max_revs)
	{
		var hist_1 = document.board.history;
		var hist_2 = []
		var k = 'nullboard.board.' + document.board.id + '.';

		for (var i=0; i<hist_1.length; i++)
		{
			var rev = hist_1[i];
			if (rev_old < rev && rev < rev_new)
				nukeBoardRevision(rev);
			else
				hist_2.push(rev);
		}
		for (var i=max_revs; i<hist_2.length; i++)
			nukeBoardRevision( hist_2[i] );

		document.board.history = loadBoardHistory(document.board.id);
	}

	//
	function nukeBoardRevision(rev)
	{
		var k = 'nullboard.board.' + document.board.id + '.' + rev;
		localStorage.removeItem(k);
		console.log("Removed " + k);
	}

	function nukeBoard()
	{
		var prefix = new RegExp('^nullboard\.board\.' + document.board.id);

		for (var i=0; i<localStorage.length; )
		{
			var k = localStorage.key(i);
			if (k.match(prefix))
			{
				console.log("Removed " + k);
				localStorage.removeItem(k);
			}
			else
			{
				i++;
			}
		}

		localStorage.removeItem('nullboard.last_board');
	}

	//
	function importBoard(blob)
	{
		var board = parseBoard(blob);
		if (! board)
		{
			alert("This doesn't appear to be a valid Nullboard board export");
			return false;
		}

		if (! confirm('Import board called "' + board.title + '", internal ID of [' + board.id + '] ?'))
			return false;

		board.id = +new Date();
		var blob_id = board.id + '.' + board.revision;

		localStorage.setItem('nullboard.board.' + blob_id, JSON.stringify(board));
		localStorage.setItem('nullboard.board.' + board.id, board.revision);

		openBoard(board.id);
	}

	//
	function createDemoBoard()
	{
		var blob =
			'{"format":20190412,"id":1555071015420,"revision":581,"title":"Welcome to Nullboard","lists":[{"title":"The Use' +
			'r Manual","notes":[{"text":"This is a note.\\nA column of notes is a list.\\nA set of lists is a board.","raw"' +
			':false,"min":false},{"text":"All data is saved locally.\\nThe whole thing works completely offline.","raw":fal' +
			'se,"min":false},{"text":"Last 50 board revisions are retained.","raw":false,"min":false},{"text":"Ctrl-Z is Un' +
			'do  -  goes one revision back.\\nCtrl-Y is Redo  -  goes one revision forward.","raw":false,"min":false},{"tex' +
			't":"Caveats","raw":true,"min":false},{"text":"Desktop-oriented.\\nMobile support is basically untested.","raw"' +
			':false,"min":false},{"text":"Works in Firefox, Chrome is supported.\\nShould work in Safari, may work in Edge.' +
			'","raw":false,"min":false},{"text":"Still very much in beta. Caveat emptor.","raw":false,"min":false},{"text":' +
			'"Issues and suggestions","raw":true,"min":false},{"text":"Post them on Github.\\nSee \\"Nullboard\\" at the to' +
			'p left for the link.","raw":false,"min":false}]},{"title":"Things to try","notes":[{"text":"\u2022   Click on ' +
			'a note to edit.","raw":false,"min":false},{"text":"\u2022   Click outside of it when done editing.\\n\u2022   ' +
			'Alternatively, use Shift-Enter.","raw":false,"min":false},{"text":"\u2022   To discard changes press Escape.",' +
			'"raw":false,"min":false},{"text":"\u2022   Try Ctrl-Enter, see what it does.\\n\u2022   Try Ctrl-Shift-Enter t' +
			'oo.","raw":false,"min":false},{"text":"\u2022   Hover over a note to show its  \u2261  menu.\\n\u2022   Hover ' +
			'over  \u2261  to reveal the options.","raw":false,"min":false},{"text":"\u2022   X  deletes the note.\\n\u2022' +
			'   R changes how a note looks.\\n\u2022   _  collapses the note.","raw":false,"min":false},{"text":"This is a ' +
			'raw note.","raw":true,"min":false},{"text":"This is a collapsed note. Only its first line is visible. Useful f' +
			'or keeping lists compact.","raw":false,"min":true}, {"text":"Links","raw":true,"min":false}, {"text":"Links pu' +
			'lse on hover and can be opened via the right-click menu  -  https://nullboard.io","raw":false,"min":false}, {"tex' +
			't":"Pressing CapsLock highlights all links and makes them left-clickable.","raw":false,"min":false}]},{"title"' +
			':"More things to try","notes":[{"text":"\u2022   Drag notes around to rearrange.\\n\u2022   Works between the ' +
			'lists too.","raw":false,"min":false},{"text":"\u2022   Click on a list name to edit.\\n\u2022   Enter to save,' +
			' Esc to cancel.","raw":false,"min":false},{"text":"\u2022   Try adding a new list.\\n\u2022   Try deleting one' +
			'. This  _can_  be undone.","raw":false,"min":false},{"text":"\u2022   Same for the board name.","raw":false,"m' +
			'in":false},{"text":"Boards","raw":true,"min":false},{"text":"\u2022   Check out   \u2261   at the top right.",' +
			'"raw":false,"min":false},{"text":"\u2022   Try adding a new board.\\n\u2022   Try switching between the boards' +
			'.","raw":false,"min":false},{"text":"\u2022   Try deleting a board. Unlike deleting a\\n     list this  _canno' +
			't_  be undone.","raw":false,"min":false},{"text":"\u2022   Export the board   (save to a file, as json)\\n' + 
			'\u2022   Import the board   (load from a save)","raw":false,"min":false}]}]}';

		var demo = parseBoard(blob);

		if (! demo)
			return false;

		demo.id = +new Date();
		demo.revision = 1;
		demo.history = [ 1 ];

		localStorage.setItem('nullboard.board.' + demo.id + '.' + demo.revision, JSON.stringify(demo));
		localStorage.setItem('nullboard.board.' + demo.id, demo.revision);
		localStorage.setItem('nullboard.last_board', demo.id);

		return demo.id;
	}

	/*
	 *
	 */
	function startEditing($text, ev)
	{
		var $note = $text.parent();
		var $edit = $note.find('.edit');

		$note[0]._collapsed = $note.hasClass('collapsed');
		$note.removeClass('collapsed');

		$edit.val( getText($text) );
		$edit.width( $text.width() );
		$edit.height( $text.height() );
		$note.addClass('editing');

		$edit.focus();
	}

	function stopEditing($edit, via_escape)
	{
		var $item = $edit.parent();
		if (! $item.hasClass('editing'))
			return;

		$item.removeClass('editing');
		if ($item[0]._collapsed)
			$item.addClass('collapsed')

		//
		var $text = $item.find('.text');
		var text_now = $edit.val().trimRight();
		var text_was = getText( $text );

		//
		var brand_new = $item.hasClass('brand-new');
		$item.removeClass('brand-new');

		if (via_escape)
		{
			if (brand_new)
			{
				$item.closest('.note, .list, .board').remove();
				return;
			}
		}
		else
		if (text_now != text_was || brand_new)
		{
			setText( $text, text_now );

			saveBoard();
			updatePageTitle();
		}

		//
		if (brand_new && $item.hasClass('list'))
			addNote($item);
	}

	//
	function addNote($list, $after, $before)
	{
		var $note  = $('tt .note').clone();
		var $notes = $list.find('.notes');

		$note.find('.text').html('');
		$note.addClass('brand-new');

		if ($before)
		{
			$before.before($note);
			$note = $before.prev();
		}
		else
		if ($after)
		{
			$after.after($note);
			$note = $after.next();
		}
		else
		{
			$notes.append($note);
			$note = $notes.find('.note').last();
		}

		$note.find('.text').click();
	}

	function deleteNote($note)
	{
		$note
		.animate({ opacity: 0 }, 'fast')
		.slideUp('fast')
		.queue(function(){
			$note.remove();
			saveBoard();
		});
	}

	//
	function addList()
	{
		var $board = $('.wrap .board');
		var $lists = $board.find('.lists');
		var $list = $('tt .list').clone();

		$list.find('.text').html('');
		$list.find('.head').addClass('brand-new');

		$lists.append($list);
		$board.find('.lists .list .head .text').last().click();

		var lists = $lists[0];
		lists.scrollLeft = Math.max(0, lists.scrollWidth - lists.clientWidth);

		setupListScrolling();
	}

	function deleteList($list)
	{
		var empty = true;

		$list.find('.note .text').each(function(){
			empty &= ($(this).html().length == 0);
		});

		if (! empty && ! confirm("Delete this list and all its notes?"))
			return;

		$list
		.animate({ opacity: 0 })
		.queue(function(){
			$list.remove();
			saveBoard();
		});

		setupListScrolling();
	}

	function moveList($list, left)
	{
		var $a = $list;
		var $b = left ? $a.prev() : $a.next();

		var $menu_a = $a.find('> .head .menu .bulk');
		var $menu_b = $b.find('> .head .menu .bulk');

		var pos_a = $a.offset().left;
		var pos_b = $b.offset().left;

		$a.css({ position: 'relative' });
		$b.css({ position: 'relative' });

		$menu_a.hide();
		$menu_b.hide();

		$a.animate({ left: (pos_b - pos_a) + 'px' }, 'fast');
		$b.animate({ left: (pos_a - pos_b) + 'px' }, 'fast', function(){

			if (left) $list.prev().before($list);
			else      $list.before($list.next());

			$a.css({ position: '', left: '' });
			$b.css({ position: '', left: '' });

			$menu_a.css({ display: '' });
			$menu_b.css({ display: '' });

			saveBoard();
		});
	}

	//
	function openBoard(board_id)
	{
		closeBoard(true);

		document.board = loadBoard(board_id);

		localStorage.setItem('nullboard.last_board', board_id);

		showBoard(true);
	}

	function reopenBoard(revision)
	{
		var board_id = document.board.id;

		var via_menu = $('.wrap .board > .head .menu .bulk').is(':visible');

		localStorage.setItem('nullboard.board.' + board_id, revision);
		openBoard(board_id);

		if (via_menu)
		{
			var $menu = $('.wrap .board > .head .menu');
			var $teaser = $menu.find('.teaser');
			var $bulk = $menu.find('.bulk');

			$teaser.hide().delay(100).queue(function(){ $(this).css('display', '').dequeue(); });
			$bulk.show().delay(100).queue(function(){ $(this).css('display', '').dequeue(); });
		}
	}

	function closeBoard(quick)
	{
		var $board = $('.wrap .board');

		if (quick)
			$board.remove();
		else
			$board
			 .animate({ opacity: 0 }, 'fast')
			 .queue(function(){ $board.remove(); });

		document.board = null;
		localStorage.setItem('nullboard.last_board', null);

		updateUndoRedo();
		updateBoardIndex();
	}

	//
	function addBoard()
	{
		document.board = new Board('');
		document.board.history = [ 0 ];

		localStorage.setItem('nullboard.last_board', document.board_id);

		showBoard(false);

		$('.wrap .board .head').addClass('brand-new');
		$('.wrap .board .head .text').click();
	}

	function deleteBoard()
	{
		var $list = $('.wrap .board .list');

		if ($list.length && ! confirm("PERMANENTLY delete this board, all its lists and their notes?"))
			return;

		nukeBoard();
		closeBoard();
	}

	//
	function undoBoard()
	{
		if (! document.board)
			return false;

		var hist = document.board.history;
		var have = document.board.revision;
		var want = 0;

		for (var i=0; i<hist.length-1 && ! want; i++)
			if (have == hist[i])
				want = hist[i+1];

		if (! want)
		{
			console.log('Undo - failed');
			return false;
		}

		console.log('Undo -> ' + want);

		reopenBoard(want);
		return true;
	}

	function redoBoard()
	{
		if (! document.board)
			return false;

		var hist = document.board.history;
		var have = document.board.revision;
		var want = 0;

		for (var i=1; i<hist.length && ! want; i++)
			if (have == hist[i])
				want = hist[i-1];

		if (! want)
		{
			console.log('Redo - failed');
			return false;
		}

		console.log('Redo -> ' + want);

		reopenBoard(want);
		return true;
	}

	/*
	 *
	 */
	function Drag()
	{
		this.item    = null;                // .text of .note
		this.priming = null;
		this.primexy = { x: 0, y: 0 };
		this.$drag   = null;
		this.mouse   = null;
		this.delta   = { x: 0, y: 0 };
		this.in_swap = false;

		this.prime = function(item, ev)
		{
			var self = this;
			this.item = item;
			this.priming = setTimeout(function(){ self.onPrimed.call(self); }, ev.altKey ? 1 : 500);
			this.primexy.x = ev.clientX;
			this.primexy.y = ev.clientY;
			this.mouse = ev;
		}

		this.cancelPriming = function()
		{
			if (this.item && this.priming)
			{
				clearTimeout(this.priming);
				this.priming = null;
				this.item = null;
			}
		}

		this.end = function()
		{
			this.cancelPriming();
			this.stopDragging();
		}

		this.isActive = function()
		{
			return this.item && (this.priming == null);
		}

		this.onPrimed = function()
		{
			clearTimeout(this.priming);
			this.priming = null;
			this.item.was_dragged = true;

			var $text = $(this.item);
			var $note = $text.parent();
			$note.addClass('dragging');

			$('body').append('<div class=dragster></div>');
			var $drag = $('body .dragster').last();

			if ($note.hasClass('collapsed'))
				$drag.addClass('collapsed');

			$drag.html( $text.html() );

			$drag.innerWidth ( $note.innerWidth()  );
			$drag.innerHeight( $note.innerHeight() );

			this.$drag = $drag;

			var $win = $(window);
			var scroll_x = $win.scrollLeft();
			var scroll_y = $win.scrollTop();

			var pos = $note.offset();
			this.delta.x = pos.left - this.mouse.clientX - scroll_x;
			this.delta.y = pos.top  - this.mouse.clientY - scroll_y;
			this.adjustDrag();

			$drag.css({ opacity: 1 });

			$('body').addClass('dragging');
		}

		this.adjustDrag = function()
		{
			if (! this.$drag)
				return;

			var $win = $(window);
			var scroll_x = $win.scrollLeft();
			var scroll_y = $win.scrollTop();

			var drag_x = this.mouse.clientX + this.delta.x + scroll_x;
			var drag_y = this.mouse.clientY + this.delta.y + scroll_y;

			this.$drag.offset({ left: drag_x, top: drag_y });

			if (this.in_swap)
				return;

			/*
			 *	see if a swap is in order
			 */
			var pos = this.$drag.offset();
			var x = pos.left + this.$drag.width()/2 - $win.scrollLeft();
			var y = pos.top + this.$drag.height()/2 - $win.scrollTop();

			var drag = this;
			var prepend = null;   // if dropping on the list header
			var target = null;    // if over some item
			var before = false;   // if should go before that item

			$(".board .list").each(function(){

				var list = this;
				var rc = list.getBoundingClientRect();
				var y_min = rc.bottom;
				var n_min = null;

				if (x <= rc.left || rc.right <= x || y <= rc.top || rc.bottom <= y)
					return;

				var $list = $(list);

				$list.find('.note').each(function(){
					var note = this;
					var rc = note.getBoundingClientRect();

					if (rc.top < y_min)
					{
						y_min = rc.top;
						n_min = note;
					}

					if (y <= rc.top || rc.bottom <= y)
						return;

					if (note == drag.item.parentNode)
						return;

					target = note;
					before = (y < (rc.top + rc.bottom)/2);
				});

				/*
				 *	dropping on the list header
				 */
				if (! target && y < y_min)
				{
					if (n_min) // non-empty list
					{
						target = n_min;
						before = true;
					}
					else
					{
						prepend = list;
					}
				}
			});

			if (! target && ! prepend)
				return;

			if (target)
			{
				if (target == drag.item.parentNode)
					return;

				if (! before && target.nextSibling == drag.item.parentNode ||
				      before && target.previousSibling == drag.item.parentNode)
					return;
			}
			else
			{
				if (prepend.firstChild == drag.item.parentNode)
					return;
			}

			/*
			 *	swap 'em
			 */
			var $have = $(this.item.parentNode);
			var $want = $have.clone();

			$want.css({ display: 'none' });

			if (target)
			{
				var $target = $(target);

				if (before)
				{
					$want.insertBefore($target);
					$want = $target.prev();
				}
				else
				{
					$want.insertAfter($target);
					$want = $target.next();
				}

				drag.item = $want.find('.text')[0];
			}
			else
			{
				var $notes = $(prepend).find('.notes');

				$notes.prepend($want);

				drag.item = $notes.find('.note .text')[0];
			}

			//
			var h = $have.height();

			drag.in_swap = true;

			$have.animate({ height: 0 }, 'fast', function(){
				$have.remove();
				$want.css({ marginTop: 5 });
				saveBoard();
			});

			$want.css({ display: 'block', height: 0, marginTop: 0 });
			$want.animate({ height: h }, 'fast', function(){
				$want.css({ opacity: '', height: '' });
				drag.in_swap = false;
				drag.adjustDrag();
			});
		}

		this.onMouseMove = function(ev)
		{
			this.mouse = ev;

			if (! this.item)
				return;

			if (this.priming)
			{
				var x = ev.clientX - this.primexy.x;
				var y = ev.clientY - this.primexy.y;
				if (x*x + y*y > 5*5)
					this.onPrimed();
			}
			else
			{
				this.adjustDrag();
			}
		}

		this.stopDragging = function()
		{
			$(this.item).parent().removeClass('dragging');
			$('body').removeClass('dragging');

			if (this.$drag)
			{
				this.$drag.remove();
				this.$drag = null;

				if (window.getSelection) { window.getSelection().removeAllRanges(); }
				else if (document.selection) { document.selection.empty(); }
			}

			this.item = null;
		}
	}

	/*
	 *
	 */
	function peekBoardTitle(board_id)
	{
		var revision = localStorage.getItem('nullboard.board.' + board_id);
		if (! revision)
			return false;

		var blob = localStorage.getItem('nullboard.board.' + board_id + '.' + revision);
		if (! blob)
			return false;

		var head = blob.indexOf(',"lists":\[{"');
		var peek = (head != -1) ? blob.substring(0, head) + '}' : blob;

		try { peek = JSON.parse(peek); } catch(x) { return false; }

		return peek && peek.title;
	}

	function updateBoardIndex()
	{
		var $index  = $('.config .boards');
		var $export = $('.config .exp-board');
		var $entry  = $('tt .load-board');

		var $board = $('.wrap .board');
		var id_now = document.board && document.board.id;
		var empty = true;

		$index.html('');
		$index.hide();

		for (var i=0; i<localStorage.length; i++)
		{
			var k = localStorage.key(i);
			var m = k.match(/^nullboard\.board\.(\d+)$/);
			if (! m)
				continue;

			var board_id = m[1];
			var title = peekBoardTitle(board_id);
			if (title === false)
				continue;

			var $e = $entry.clone();
			$e[0].board_id = board_id;
			$e.html( title || '(unnamed board)' );

			if (board_id == id_now)
				$e.addClass('active');

			$index.append($e);
			empty = false;
		}

		if (id_now) $export.show();
		else        $export.hide();

		if (! empty) $index.show();
	}

	//
	function showDing()
	{
		$('body')
		.addClass('ding')
		.delay(100)
		.queue(function(){ $(this).removeClass('ding').dequeue(); });
	}

	//
	function showOverlay($overlay, $div)
	{
		$overlay
		.html('')
		.append($div)
		.css({ opacity: 0, display: 'block' })
		.animate({ opacity: 1 });
	}

	function hideOverlay($overlay)
	{
		$overlay.animate({ opacity: 0 }, function(){
			$overlay.hide();
		});
	}

	//
	function formatLicense()
	{
		var text = document.head.childNodes[1].nodeValue;
		var pos = text.search('LICENSE');
		var qos = text.search('Software:');
		var bulk;

		bulk = text.substr(pos, qos-pos);
		bulk = bulk.replace(/([^\n])\n\t/g, '$1 ');
		bulk = bulk.replace(/\n\n\t/g, '\n\n');
		bulk = bulk.replace(/([A-Z ]{7,})/g, '<u>$1</u>');

		//
		var c1 = [];
		var c2 = [];

		text.substr(qos).trim().split('\n').forEach(function(line){
			line = line.split(':');
			c1.push( line[0].trim() + ':' );
			c2.push( line[1].trim() );
		});

		bulk += '<span>' + c1.join('<br>') + '</span>';
		bulk += '<span>' + c2.join('<br>') + '</span>';

		//
		var links =
		[
			{ text: '2-clause BSD license', href: 'https://opensource.org/licenses/BSD-2-Clause/' },
			{ text: 'Commons Clause',       href: 'https://commonsclause.com/' }
		];

		links.forEach(function(l){
			bulk = bulk.replace(l.text, '<a href="' + l.href + '" target=_blank>' + l.text + '</a>');
		});

		return bulk.trim();
	}

	/*
	 *
	 */
	var drag = new Drag();

	//
	function setRevealState(ev)
	{
		var raw = ev.originalEvent;
		var caps = raw.getModifierState && raw.getModifierState( 'CapsLock' );

		if (caps) $('body').addClass('reveal');
		else      $('body').removeClass('reveal');
	}

	$(window).live('blur', function(){
		$('body').removeClass('reveal');
	});

	$(document).live('keydown', function(ev){
		setRevealState(ev);
	});

	$(document).live('keyup', function(ev){

		var raw = ev.originalEvent;

		setRevealState(ev);

		if (ev.target.nodeName == 'TEXTAREA' ||
		    ev.target.nodeName == 'INPUT')
			return;

		if (ev.ctrlKey && (raw.code == 'KeyZ'))
		{
			var ok = ev.shiftKey ? redoBoard() : undoBoard();
			if (! ok)
				showDing();
		}
		else
		if (ev.ctrlKey && (raw.code == 'KeyY'))
		{
			if (! redoBoard())
				showDing();
		}
	});

	$('.board .text').live('click', function(ev){

		if (this.was_dragged)
		{
			this.was_dragged = false;
			return false;
		}

		drag.cancelPriming();

		startEditing($(this), ev);
		return false;
	});

	$('.board .note .text a').live('click', function(ev){

		if (! $('body').hasClass('reveal'))
			return true;

		ev.stopPropagation();
		return true;
	});

	//
	function handleTab(ev)
	{
		var $this = $(this);
		var $note = $this.closest('.note');
		var $sibl = ev.shiftKey ? $note.prev() : $note.next();

		if ($sibl.length)
		{
			stopEditing($this, false);
			$sibl.find('.text').click();
		}
	}

	$('.board .edit').live('keydown', function(ev){

		// esc
		if (ev.keyCode == 27)
		{
			stopEditing($(this), true);
			return false;
		}

		// tab
		if (ev.keyCode == 9)
		{
			handleTab.call(this, ev);
			return false;
		}

		// enter
		if (ev.keyCode == 13 && ev.ctrlKey)
		{
			var $this = $(this);
			var $note = $this.closest('.note');
			var $list = $note.closest('.list');

			stopEditing($this, false);

			if ($note && ev.shiftKey) // ctrl-shift-enter
				addNote($list, null, $note);
			else
			if ($note && !ev.shiftKey) // ctrl-enter
				addNote($list, $note);

			return false;
		}

		if (ev.keyCode == 13 && this.tagName == 'INPUT' ||
		    ev.keyCode == 13 && ev.altKey ||
		    ev.keyCode == 13 && ev.shiftKey)
		{
			stopEditing($(this), false);
			return false;
		}

		//
		if (ev.key == '*' && ev.ctrlKey)
		{
			var have = this.value;
			var pos  = this.selectionStart;
			var want = have.substr(0, pos) + '\u2022 ' + have.substr(this.selectionEnd);
			$(this).val(want);
			this.selectionStart = this.selectionEnd = pos + 2;
			return false;
		}

		return true;
	});

	$('.board .edit').live('keypress', function(ev){

		// tab
		if (ev.keyCode == 9)
		{
			handleTab.call(this, ev);
			return false;
		}
	});

	//
	$('.board .edit').live('blur', function(ev){
		if (document.activeElement != this)
			stopEditing($(this));
		else
			; // switch away from the browser window
	});

	//
	$('.board .note .edit').live('input propertychange', function(){

		var delta = $(this).outerHeight() - $(this).height();

		$(this).height(10);

		if (this.scrollHeight > this.clientHeight)
			$(this).height(this.scrollHeight-delta);

	});

	//
	$('.config .add-board').live('click', function(){
		addBoard();
		return false;
	});

	$('.config .load-board').live('click', function(){

		var board_id = $(this)[0].board_id;
		if ((document.board && document.board.id) == board_id)
			closeBoard();
		else
			openBoard( $(this)[0].board_id );

		return false;
	});

	$('.board .del-board').live('click', function(){
		deleteBoard();
		return false;
	});

	$('.board .undo-board').live('click', function(){
		undoBoard();
		return false;
	});

	$('.board .redo-board').live('click', function(){
		redoBoard();
		return false;
	});

	//
	$('.board .add-list').live('click', function(){
		addList();
		return false;
	});

	$('.board .del-list').live('click', function(){
		deleteList( $(this).closest('.list') );
		return false;
	});

	$('.board .mov-list-l').live('click', function(){
		moveList( $(this).closest('.list'), true );
		return false;
	});

	$('.board .mov-list-r').live('click', function(){
		moveList( $(this).closest('.list'), false );
		return false;
	});

	//
	$('.board .add-note').live('click', function(){
		addNote( $(this).closest('.list') );
		return false;
	});

	$('.board .del-note').live('click', function(){
		deleteNote( $(this).closest('.note') );
		return false;
	});

	$('.board .raw-note').live('click', function(){
		$(this).closest('.note').toggleClass('raw');
		saveBoard();
		return false;
	});

	$('.board .collapse').live('click', function(){
		$(this).closest('.note').toggleClass('collapsed');
		saveBoard();
		return false;
	});

	//
	$('.board .note .text').live('mousedown', function(ev){
		drag.prime(this, ev);
	});

	$(document).on('mouseup', function(ev){
		drag.end();
	});

	$(document).on('mousemove', function(ev){
		setRevealState(ev);
		drag.onMouseMove(ev);
	});

	//
	$('.config .imp-board').click(function(ev){
		$('.config .imp-board-select').click();
		return false;
	});

	$('.config .imp-board-select').change(function(){
		var files = this.files;
		var reader = new FileReader();
		reader.onload = function(ev){ importBoard(ev.target.result); };
		reader.readAsText(files[0]);
		return true;
	});

	$('.config .exp-board').click(function(){
		var board = document.board;
		var revision = localStorage.getItem('nullboard.board.' + board.id);
		if (! revision)
			return false;

		var blob = localStorage.getItem('nullboard.board.' + board.id + '.' + revision);
		if (! blob)
			return false;

		var file = 'Nullboard-' + board.id + '-' + board.title + '.nbx';

		blob = encodeURIComponent(blob);
		blob = "data:application/octet-stream," + blob;

		$(this).attr('href', blob);
		$(this).attr('download', file);
		return true;
	});

	$('.config .switch-theme').click(function() {
		var $body = $('body');
		$body.toggleClass('dark');
		localStorage.setItem('nullboard.theme', $body.hasClass('dark') ? 'dark' : '');
		return false;
	});

	$('.config .switch-fsize').click(function(){
		var $body = $('body');
		$body.toggleClass('z1');
		localStorage.setItem('nullboard.fsize', $body.hasClass('z1') ? 'z1' : '');
		return false;
	});

	//
	var $overlay = $('.overlay');

	$overlay.click(function(ev){
		if (ev.originalEvent.target != this)
			return true;
		hideOverlay($overlay);
		return false;
	});

	$(window).keydown(function(ev){
		if ($overlay.css('display') != 'none' && ev.keyCode == 27)
			hideOverlay($overlay);
	});

	$('.view-about').click(function(){
		var $div = $('tt .about').clone();
		$div.find('div').html('Version ' + nb_codeVersion);
		showOverlay($overlay, $div);
		return false;
	});

	$('.view-license').click(function(){

		var $div = $('tt .license').clone();
		$div.html(formatLicense());
		showOverlay($overlay, $div);
		return false;
	});

	//
	function adjustLayout()
	{
		var $body = $('body');
		var $board = $('.board');

		if (! $board.length)
			return;

		var lists = $board.find('.list').length;
		var lists_w = (lists < 2) ? 250 : 260 * lists - 10;
		var body_w = $body.width();

		if (lists_w + 190 <= body_w)
		{
			$board.css('max-width', '');
			$body.removeClass('crowded');
		}
		else
		{
			var max = Math.floor( (body_w - 40) / 260 );
			max = (max < 2) ? 250 : 260 * max - 10;
			$board.css('max-width', max + 'px');
			$body.addClass('crowded');
		}
	}

	$(window).resize(adjustLayout);

	adjustLayout();

	//
	function adjustListScroller()
	{
		var $board = $('.board');
		if (! $board.length)
			return;

		var $lists    = $('.board .lists');
		var $scroller = $('.board .lists-scroller');
		var $inner    = $scroller.find('div');

		var max  = $board.width();
		var want = $lists[0].scrollWidth;
		var have = $inner.width();

		if (want <= max)
		{
			$scroller.hide();
			return;
		}

		$scroller.show();
		if (want == have)
			return;

		$inner.width(want);
		cloneScrollPos($lists, $scroller);
	}

	//
	function cloneScrollPos($src, $dst)
	{
		var src = $src[0];
		var dst = $dst[0];

		if (src._busyScrolling)
		{
			src._busyScrolling--;
			return;
		}

		dst._busyScrolling++;
		dst.scrollLeft = src.scrollLeft;
	}

	function setupListScrolling()
	{
		var $lists    = $('.board .lists');
		var $scroller = $('.board .lists-scroller');

		adjustListScroller();

		$lists[0]._busyScrolling = 0;
		$scroller[0]._busyScrolling = 0;

		$scroller.on('scroll', function(){ cloneScrollPos($scroller, $lists); });
		$lists   .on('scroll', function(){ cloneScrollPos($lists, $scroller); });

		adjustLayout();
	}

	//
	if (localStorage.getItem('nullboard.theme') == 'dark')
		$('body').addClass('dark');

	if (localStorage.getItem('nullboard.fsize') == 'z1')
		$('body').addClass('z1');

	//
	var board_id = localStorage.getItem('nullboard.last_board');

	if (board_id)
		document.board = loadBoard(board_id);

	updateBoardIndex();

	if (! document.board && ! $('.config .load-board').length)
	{
		var demo_id = createDemoBoard();
		document.board = loadBoard(demo_id);
		updateBoardIndex();
	}

	if (document.board)
	{
		showBoard(true);
	}

	//
	setInterval(adjustListScroller, 100);

	setupListScrolling();

</script>

</html>