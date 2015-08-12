<!DOCTYPE html>
<html>
	<head>
		<title>New York Times Daily Crossword</title>
		<meta name="GENERATOR" content="Microsoft Visual Studio .NET 7.1">
		<meta name="vs_targetSchema" content="http://schemas.microsoft.com/intellisense/ie5">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="initial-scale=1">

		<style type="text/css">
			*{user-select:none;cursor: default;}
			.vblack {background-color:#000000;visibility:visible;}
			.num{height:6pt;font-family:arial;font-size:6pt;visibility:visible}
			.letter{height:100%;width:100%;font-family:helvetica;font-size:8pt;text-align:center;display:none;}
			.subst{font-family:verdana;font-size:8pt;text-align:center;display:normal;}
			.subst2{font-family:verdana;font-size:8pt;text-align:center;display:normal;}
			.copy{font-family:Arial;font-weight:bold;font-size:7pt;}
			.PuzTable {margin-bottom:0px;border-collapse:collapse;}
			.PuzTable td {vertical-align:top;height:24px;width:24px;border:1px solid black;padding:0px;}
			.PuzTitle{font-family:Arial;font-weight:normal;font-size:8pt;}
			.puzzle-selection
			{
				z-index:1000;
				width:250px;
				font-family:Arial;
				font-size:9pt;
				background-color:white;
				border:1px solid black;
				padding:5px;
				margin-bottom:10px;
				box-shadow: 5px 5px 5px gray;
			}
			.puzzle-selection *{vertical-align: middle;}
			.puzzle-selection .date-picker{width: 105px;}
			.puzzle-selection .ctrl-container{margin-top:5px;}
			.puzzle-clues a{visibility:hidden;}
			.puzzle-clues-across, .puzzle-clues-down, td
			{
				font-family:Arial;
				font-size:7pt;
				vertical-align:top;
			}
			.puzzle-across-header
			{
				font-size:9pt;
				font-weight:bold;
				padding:10px 0px 5px 0px;
			}
			#dlgPuzzSel
			{
				z-index:1000;
				width:250px;
				font-family:Arial;
				font-size:9pt;
				background-color:white;
				border:1px solid black;
				padding:7px;
				margin-bottom:10px;
				box-shadow: 5px 5px 5px gray;
			}
			@media print
			{
				#dlgPuzzSel{display: none;}
			}
		</style>

		<link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jqueryui/1.11.4/themes/smoothness/jquery-ui.css">
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.js"></script>
		<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.11.4/jquery-ui.js"></script>

		<link rel="stylesheet" href="./widget_box/widget_box.css">
		<script src="./widget_box/widget_box.js"></script>

		<script type="text/javascript">
			sprintf = function (sPattern, args) {
				var _args = arguments;
				return sPattern && sPattern.replace(/\%(\d+)/g, function (s, n) {
					return _args[n];
				});
			};
			$(function()
			{
				function LoadPuzzle(date)
				{
					var strParm = sprintf("http://www.xwordinfo.com/Crossword?date=%1", $.datepicker.formatDate("mm/dd/yy", date));
					var strUri = "./proxy.jsp?" + strParm;
					var ret = $.ajax(
					{
						"url": strUri,
						"dataType":"html",
						"success":function(puzzlePage)
						{
							puzzlePage = puzzlePage.replace(/ : <a/g, "<a");
							var $puzzPage = $(puzzlePage);
							var $puzzTab = $puzzPage.find("#CPHContent_PuzTable");
							var $puzzTitle = $puzzPage.find("#CPHContent_TitleLabel").addClass("PuzTitle");
							var $puzzCRight = $puzzPage.find("#CPHContent_Copyright");
							var $puzzSelection = $(".puzzle-selection");
							var $puzzContainer = $(".puzzle-container").empty();
							$puzzContainer.append($puzzTitle);
							$puzzContainer.append($puzzTab);
							$puzzContainer.append($puzzCRight);
							
							var $puzzClues = $(".puzzle-clues");
							var $cluesAcross = $(".puzzle-clues-across").append($puzzPage.find("#CPHContent_AcrossClues"));
							var $cluesDown = $(".puzzle-clues-down").append($puzzPage.find("#CPHContent_DownClues"));
						
							var $puzzlePageContainer = $(".puzzle-page-container");
						},
						"error":function(xhr, strError, strMessage)
						{
							$(document.documentElement).html(xhr.responseText);
						}
					});
				}
				var date = new Date();
				$("#puzzDate").datepicker().datepicker("setDate", date).on("change", function(e)
				{
					var date = $(e.target).datepicker("getDate");
					LoadPuzzle(date);
				});
				LoadPuzzle(date);
				$("#makeDiagramless").on("change", function(e)
				{
					$(".vblack").css("backgroundColor", (this.checked) ? "white" : "black");
					var idx = $(".num").css("color", (this.checked) ? "white" : "black").index();
				});
				
				$("#showAnswers").on("change", function(e)
				{
					$(".letter").css("display", (this.checked) ? "inherit" : "none");
				});
				$("#btnPuzzPrint").on("click", function(e)
				{
					window.print();
				});
				var dlgPuzzSel = $("#dlgPuzzSel").css({"height":"auto", "width":"300px"}).draggable();
				var puzzDateLabel = $("#puzzDateLabel").css({"marginRight":"3px"});
				var boxDateOptions = $("#boxDateOptions").css({"marginBottom":"7px", "border":"0px solid red"});
				var puzzDate = $("#puzzDate").css({"minWidth":"20px", "marginRight":"7px"});
				var puzzPrint = $("#btnPuzzPrint");
				var boxMoreOptions = $("#boxMoreOptions").css({"border":"0px solid green"});
				var showAnswersLabel = $("#showAnswersLabel").css({"margin-right": "7px"});
			});
		</script>
	</head>
	<body>
		<div class="puzzle-page-container">
			<div id="dlgPuzzSel" data-role-flex="box" data-flex-direction="column" data-flex-justify="center" data-flex-align-items="stretch">
				<div id="boxDateOptions" data-role-flex="box, item" data-flex-grow="1" data-flex-align-items="center">
					<label id="puzzDateLabel" for="puzzDate" data-role-flex="item">Puzzle Date:</label>
					<input id="puzzDate" data-role-flex="item" data-flex-shrink="1" data-flex-grow="1" type="text" readonly="">
					<button id="btnPuzzPrint" data-role-flex="item">Print...</button>
				</div>
				<div id="boxMoreOptions" data-role-flex="box, item" data-flex-grow="1" data-flex-align-items="center" data-flex-align-self="flex-end">
					<input id="showAnswers" type="checkbox" data-role-flex="item"/>
					<label id="showAnswersLabel" for="showAnswers" data-role-flex="item">Anwsers</label>
					<input id="makeDiagramless" type="checkbox" data-role-flex="item"/>
					<label for="makeDiagramless" data-role-flex="item">Diagramless</label>
				</div>
			</div>

			<div class="puzzle-container">
			</div>

			<table class="puzzle-clues">
				<tr>
					<td>
						<div class="puzzle-clues-across">
							<div class="puzzle-across-header">Across</div>
						</div>
					</td>
					<td>
						<div class="puzzle-clues-down">
							<div class="puzzle-across-header">Down</div>
						</div>
					</td>
				</tr>
			</table>
		</div>
	</body>
</html>
