// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © Jager1703

//@version=5

indicator("RedPillFX",shorttitle="RedPillFX",overlay=true, max_bars_back=1000, max_labels_count=500, max_lines_count=500, max_boxes_count=500)


///////////////////////////////////////////////////////////


var GRP1 = "DON'T TOUCH XD"

timeZone = input.string("GMT-4", title="Time Zone",group=GRP1)


////////////////////////////////////////////////////////////


var GRP40="Notes"


Table2 = input.bool(defval=true,title="Show Notes", group=GRP40)
TableBG2 = input.color(defval=color.new(color.blue, 50), title= "Color Of Background",inline='line1', group=GRP40)
Tab2txtCol = input.color(defval=color.new(color.white, 0),title="Color Of Text", inline='line2', group=GRP40)
sizeText = input.string(defval='normal',title='Text Size', options=['tiny', 'small', 'normal', 'large', 'huge', 'auto'],inline='line3', group=GRP40)
TabOption2 = input.string(defval="Middle Right", title="Choose Location", options=["Top Left", "Top Center", "Top Right", "Middle Left", "Middle Right", "Bottom Left", "Bottom Center", "Bottom Right"], inline="TAB2", group=GRP40)
tabinp2 = TabOption2== "Top Left" ? position.top_left : TabOption2 == "Top Center" ? position.top_center : TabOption2 == "Top Right" ? position.top_right : TabOption2 == "Middle Left" ? position.middle_left : TabOption2 == "Middle Right" ? position.middle_right : TabOption2 == "Bottom Left" ? position.bottom_left : TabOption2 == "Bottom Center" ? position.bottom_center : position.bottom_right
notes = input.text_area("Wright Here", "Notes", group = GRP40)


/////////////////////////////////////////////////////////


var GRP2 = "ASIAN SESSION"


SessionOpen(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float sessionOpen = na

    if insideSession and not insideSession[1]
        sessionOpen := low
    else if insideSession
        sessionOpen := math.min(sessionOpen, low)
    
    sessionOpen
    
SessionClose(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float sessionClose = na

    if insideSession and not insideSession[1]
        sessionClose := high
    else if insideSession
        sessionClose := math.max(sessionClose, high)
    
    sessionClose
 
InSession(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))


sessionOpen  = input.session("1800-1801", title="Begin Asian Session",group=GRP2)
sessionClose  = input.session("0000-0001", title="End Asian Session",group=GRP2)



sessOpen = SessionOpen(sessionOpen, timeZone)
sessClose = SessionClose(sessionClose, timeZone)


showLines = input(title='Show Lines', defval=true, group=GRP2)
showBackground = input(title='Show Background', defval=true, group=GRP2)
showMiddleLine = input(title='Show Middle Line', defval=true, group=GRP2)
rangeTime = input.session(title="Session Time", defval='1800-0001', group=GRP2)
extendTime = input.session(title='Extend Until', defval='0000-0501', group=GRP2)
linesWidth = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP2)
lineColor0 = input(color.new(color.white,100),"Don't Matter",group=GRP2)
if (sessOpen > sessClose)
    lineColor0 := input(color.new(color.red, 80), 'Bearish Asian Line', group=GRP2)
else
    lineColor0 := input(color.new(color.green, 80), 'Bullish Asian Line', group=GRP2)
backgroundColor = input(color.new(color.white,100),"Don't Matter",group=GRP2)
if (sessOpen > sessClose)
    backgroundColor := input(color.new(color.red, 80), 'Bearish Asian Box', group=GRP2)
else
    backgroundColor := input(color.new(color.green, 80), 'Bullish Asian Box', group=GRP2)


///////////////////////////////////////////////////////////


var GRP3 = "Asian DTCC"


SessionOpen1(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession1 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float sessionOpen1 = na

    if insideSession1 and not insideSession1[1]
        sessionOpen1 := low
    else if insideSession1
        sessionOpen1 := math.min(sessionOpen1, low)
    
    sessionOpen1
    
SessionClose1(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession1 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float sessionClose1 = na

    if insideSession1 and not insideSession1[1]
        sessionClose1 := high
    else if insideSession1
        sessionClose1 := math.max(sessionClose1, high)
    
    sessionClose1


InSession1(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))


sessionOpen1  = input.session("2000-2001", title="Begin DTCC",group=GRP3)
sessionClose1  = input.session("2045-2046", title="End DTCC",group=GRP3)


sessOpen1 = SessionOpen1(sessionOpen1, timeZone)
sessClose1 = SessionClose1(sessionClose1, timeZone)

showLines1 = input(title='Show Lines', defval=true, group=GRP3)
showBackground1 = input(title='Show Background', defval=true, group=GRP3)
showMiddleLine1 = input(title='Show Middle Line', defval=true, group=GRP3)
rangeTime1 = input.session(title="Time DTCC", defval='2000-2046', group=GRP3)
extendTime1 = input.session(title='Extend Until', defval='2000-0500', group=GRP3)
linesWidth1 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP3)
lineColor1 = input(color.new(color.white,100),"Don't Matter",group=GRP3)
if (sessOpen1 > sessClose1)
    lineColor1 := input(color.new(color.red, 70), 'Bearish DTCC Lines ', group=GRP3)
else
    lineColor1 := input(color.new(color.green, 70), 'Bullish DTCC Lines', group=GRP3)

backgroundColor1 = input(color.new(color.white,100),"Don't Matter",group=GRP3)
if (sessOpen1 > sessClose1)
    backgroundColor1 := input(color.new(color.red, 70), 'Bearish DTCC ', group=GRP3)
else
    backgroundColor1 := input(color.new(color.green, 70), 'Bullish DTCC', group=GRP3)


////////////////////////////////////////////////////////////


//Midnight Open

var GRP20 = "Midnight Open"

MidnightOpen(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession4 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float midOpen = na

    if insideSession4 and not insideSession4[1]
        midOpen := open
    else if insideSession4
        midOpen := math.max(midOpen, open)
    
    midOpen
 
InSession4(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))


midOpen  = input.session("0000-0001", title="Midnight Open",group=GRP20)


midnightO = MidnightOpen(midOpen, timeZone)


showLines4 = input(title='Show Lines', defval=true, group=GRP20)
LineColor4 = input(color.new(color.green, 50), 'Middle Line Color', group=GRP20)
extendTime4 = input.session(title="Extend Until", defval='0000-1631', group=GRP20)
linesWidth4 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP20)


///////////////////////////////////////////////////////


var GRP4 = "FRANKFURT SESSION"


showBackground2 = input(title='Show Background', defval=true, group=GRP4)
rangeTime2 = input.session(title='Session Time', defval='0200-0301', group=GRP4)
backgroundColor2 = input(color.new(color.yellow, 80), 'Box Background Color', group=GRP4)


//////////////////////////////////////////////////////
//London Session


var GRP5= "LONDON SESSION"


showBackground6 = input(title='Show Background', defval=true, group=GRP5)
rangeTime6 = input.session(title='Session Time', defval='0300-0501', group=GRP5)
backgroundColor6 = input(color.new(color.silver, 80), 'Box Background Color', group=GRP5)

/////////////////////////////////////////////////////////////////////

var GRP6 = "NEW YORK TRAP"


showBackground3 = input(title='Show Background', defval=true, group=GRP6)
rangeTime3 = input.session(title='Session Time', defval='0545-0731', group=GRP6)
backgroundColor3 = input(color.new(color.red, 80), 'Box Background Color', group=GRP6)


////////////////////////////////////////////
//NY Session


var GRP7= "NEW YORK SESSION"


showBackground7 = input(title='Show Background', defval=true, group=GRP7)
rangeTime7 = input.session(title='Session Time', defval='0700-0931', group=GRP7)
backgroundColor7 = input(color.new(color.blue, 80), 'Box Background Color', group=GRP7)

//////////////////////////////////////////

//Tracer Boxe et Ligne NY Session


inSession = not na(time(timeframe.period, rangeTime))
inExtend = not na(time(timeframe.period, extendTime))


startTime = 0
startTime := inSession and not inSession[1] ? time : startTime[1]

//Box lines
var line lowHLine = na
var line topHLine = na
var line leftVLine = na
var line rightVLine = na
var line middleHLine = na
var box bgBox = na

var low_val = 0.0
var high_val = 0.0
if inSession and not inSession[1]
    low_val := low
    high_val := high
    high_val

// Plot lines
if inSession and timeframe.isintraday
    if inSession[1]
        line.delete(lowHLine)
        line.delete(topHLine)
        line.delete(leftVLine)
        line.delete(rightVLine)
		line.delete(middleHLine)
        box.delete(bgBox)

    if low < low_val
        low_val := low
        low_val
    if high > high_val
        high_val := high
        high_val

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground
        bgBox := box.new(startTime, high_val, time, low_val, xloc=xloc.bar_time, bgcolor=backgroundColor, border_width=0)
    if showLines
        lowHLine := line.new(startTime, low_val, time, low_val, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
        topHLine := line.new(startTime, high_val, time, high_val, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
        leftVLine := line.new(startTime, high_val, startTime, low_val, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
        rightVLine := line.new(time, high_val, time, low_val, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)

    //Create Middle line
    if showMiddleLine
        middleHLine := line.new(startTime, (high_val + low_val) / 2, time, (high_val + low_val) / 2, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)

else
    if inExtend and showLines and not inSession and timeframe.isintraday
        time1 = line.get_x1(lowHLine)
        time2 = line.get_x2(lowHLine)
        price = line.get_y1(lowHLine)
        line.delete(lowHLine)
        lowHLine := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
        
        time1 := line.get_x1(topHLine)
        time2 := line.get_x2(topHLine)
        price := line.get_y1(topHLine)
        line.delete(topHLine)
        topHLine := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
    if inExtend and showMiddleLine and not inSession and timeframe.isintraday
        time1 = line.get_x1(middleHLine)
        time2 = line.get_x2(middleHLine)
        price = line.get_y1(middleHLine)
        line.delete(middleHLine)
        middleHLine := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor0, style=line.style_solid, width=linesWidth)
        middleHLine


////////////////////////////////////////////


//Tracer Boxe et Ligne du Asian DTCC


////////////////////////////////////////
	
inSession1 = not na(time(timeframe.period, rangeTime1))
inExtend1 = not na(time(timeframe.period, extendTime1))

startTime1 = 0
startTime1 := inSession1 and not inSession1[1] ? time : startTime1[1]

//Box lines
var line lowHLine1 = na
var line topHLine1 = na
var line leftVLine1 = na
var line rightVLine1 = na
var line middleHLine1 = na
var box bgBox1 = na

var low_val1 = 0.0
var high_val1 = 0.0
if inSession1 and not inSession1[1]
    low_val1 := low
    high_val1 := high
    high_val1

// Plot lines
if inSession1 and timeframe.isintraday
    if inSession1[1]
        line.delete(lowHLine1)
        line.delete(topHLine1)
        line.delete(leftVLine1)
        line.delete(rightVLine1)
        line.delete(middleHLine1)
        box.delete(bgBox1)

    if low < low_val1
        low_val1 := low
        low_val1
    if high > high_val1
        high_val1 := high
        high_val1

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground1
        bgBox1 := box.new(startTime1, high_val1, time, low_val1, xloc=xloc.bar_time, bgcolor=backgroundColor1, border_width=0)
	
	if showLines1
        lowHLine1 := line.new(startTime1, low_val1, time, low_val1, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
        topHLine1 := line.new(startTime1, high_val1, time, high_val1, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
        leftVLine1 := line.new(startTime1, high_val1, startTime1, low_val1, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
        rightVLine1 := line.new(time, high_val1, time, low_val1, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)

    //Create Middle line
    if showMiddleLine1
        middleHLine1 := line.new(startTime1, (high_val1 + low_val1) / 2, time, (high_val1 + low_val1) / 2, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)

else
    if inExtend1 and showLines1 and not inSession1 and timeframe.isintraday
        time1 = line.get_x1(lowHLine1)
        time2 = line.get_x2(lowHLine1)
        price = line.get_y1(lowHLine1)
        line.delete(lowHLine1)
        lowHLine1 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
        
        time1 := line.get_x1(topHLine1)
        time2 := line.get_x2(topHLine1)
        price := line.get_y1(topHLine1)
        line.delete(topHLine1)
        topHLine1 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
    if inExtend1 and showMiddleLine1 and not inSession1 and timeframe.isintraday
        time1 = line.get_x1(middleHLine1)
        time2 = line.get_x2(middleHLine1)
        price = line.get_y1(middleHLine1)
        line.delete(middleHLine1)
        middleHLine1 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=lineColor1, style=line.style_solid, width=linesWidth1)
        middleHLine1

//////////////////////////////////////////////////////////////////


//Tracer Boxe et Ligne de Frankfurt


inSession2 = not na(time(timeframe.period, rangeTime2))

startTime2 = 0
startTime2 := inSession2 and not inSession2[1] ? time : startTime2[1]

//Box lines

var line lowHLine2 = na
var line topHLine2 = na
var line leftVLine2 = na
var line rightVLine2 = na
var line middleHLine2 = na
var box bgBox2 = na

var low_val2 = 0.0
var high_val2 = 0.0
if inSession2 and not inSession2[1]
    low_val2 := low
    high_val2 := high
    high_val2

// Plot lines

if inSession2 and timeframe.isintraday
    if inSession2[1]
        line.delete(lowHLine2)
        line.delete(topHLine2)
        line.delete(leftVLine2)
        line.delete(rightVLine2)
        line.delete(middleHLine2)
        box.delete(bgBox2)

    if low < low_val2
        low_val2 := low
        low_val2
    if high > high_val2
        high_val2 := high
        high_val2

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground2
        bgBox2 := box.new(startTime2, high_val2, time, low_val2, xloc=xloc.bar_time, bgcolor=backgroundColor2, border_width=0)


////////////////////////////////////////////


//Tracer Boxe et Ligne du NY TRAP


inSession6 = not na(time(timeframe.period, rangeTime6))

startTime6 = 0
startTime6 := inSession6 and not inSession6[1] ? time : startTime6[1]

//Box lines

var line lowHLine6 = na
var line topHLine6 = na
var line leftVLine6 = na
var line rightVLine6 = na
var line middleHLine6 = na
var box bgBox6 = na

var low_val6 = 0.0
var high_val6 = 0.0
if inSession6 and not inSession6[1]
    low_val6 := low
    high_val6 := high
    high_val6

// Plot lines

if inSession6 and timeframe.isintraday
    if inSession6[1]
        line.delete(lowHLine6)
        line.delete(topHLine6)
        line.delete(leftVLine6)
        line.delete(rightVLine6)
        line.delete(middleHLine6)
        box.delete(bgBox6)

    if low < low_val6
        low_val6 := low
        low_val6
    if high > high_val6
        high_val6 := high
        high_val6

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground6
        bgBox6 := box.new(startTime6, high_val6, time, low_val6, xloc=xloc.bar_time, bgcolor=backgroundColor6, border_width=0)

///////////////////////////////////////////////////////////////////////


//Tracer Boxe et Ligne du NY TRAP


inSession3 = not na(time(timeframe.period, rangeTime3))

startTime3 = 0
startTime3 := inSession3 and not inSession3[1] ? time : startTime3[1]

//Box lines

var line lowHLine3 = na
var line topHLine3 = na
var line leftVLine3 = na
var line rightVLine3 = na
var line middleHLine3 = na
var box bgBox3 = na

var low_val3 = 0.0
var high_val3 = 0.0
if inSession3 and not inSession3[1]
    low_val3 := low
    high_val3 := high
    high_val3

// Plot lines

if inSession3 and timeframe.isintraday
    if inSession3[1]
        line.delete(lowHLine3)
        line.delete(topHLine3)
        line.delete(leftVLine3)
        line.delete(rightVLine3)
        line.delete(middleHLine3)
        box.delete(bgBox3)

    if low < low_val3
        low_val3 := low
        low_val3
    if high > high_val3
        high_val3 := high
        high_val3

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground3
        bgBox3 := box.new(startTime3, high_val3, time, low_val3, xloc=xloc.bar_time, bgcolor=backgroundColor3, border_width=0)



///////////////////////////////////////////////

//Tracer Ligne Midnight


inSession4 = not na(time(timeframe.period, midOpen))
inExtend4 = not na(time(timeframe.period, extendTime4))


startTime4 = 0
startTime4 := inSession4 and not inSession4[1] ? time : startTime4[1]

var line lowHLine4 = na

var low_val4 = 0.0
if inSession4 and not inSession4[1]
    low_val4 := midnightO

// Plot lines

if inSession4 and timeframe.isintraday
    if inSession4[1]
        line.delete(lowHLine4)
    if low < low_val4
        low_val4 := midnightO
        low_val4


    //x1, y1, x2, y2 
    if showLines4
        lowHLine4 := line.new(startTime4, low_val4, time, low_val4, xloc=xloc.bar_time, color=LineColor4, style=line.style_solid,width=linesWidth4)


else
    if inExtend4 and showLines4 and not inSession4 and timeframe.isintraday
        time1 = line.get_x1(lowHLine4)
        time2 = line.get_x2(lowHLine4)
        price = line.get_y1(lowHLine4)
        line.delete(lowHLine4)
        lowHLine4 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=LineColor4, style=line.style_solid,width=linesWidth4)




///////////////////////////////////////////


//Frankurt Open


frankfurtOpen(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession5 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float frOpen = na

    if insideSession5 and not insideSession5[1]
        frOpen := open
    else if insideSession5
        frOpen := math.max(frOpen, open)
    
    frOpen
 
InSession5(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))


frOpen  = input.session("0200-0201", title="Franfurt Open",group=GRP4)


FrankfurtO = frankfurtOpen(frOpen, timeZone)


showLines5 = input(title='Show Line', defval=true, group=GRP4)
LineColor5 = input(color.new(color.yellow, 50), 'Open Line Color', group=GRP4)
extendTime5 = input.session(title="Extend Until", defval='0200-1631', group=GRP4)
linesWidth5 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP4)


//Tracer Ligne Midnight


inSession5 = not na(time(timeframe.period, frOpen))
inExtend5 = not na(time(timeframe.period, extendTime5))

startTime5 = 0
startTime5 := inSession5 and not inSession5[1] ? time : startTime5[1]

var line lowHLine5 = na

var low_val5 = 0.0
if inSession5 and not inSession5[1]
    low_val5 := FrankfurtO

// Plot lines

if inSession5 and timeframe.isintraday
    if inSession5[1]
        line.delete(lowHLine5)
    if low < low_val5
        low_val5 := FrankfurtO
        low_val5


    //x1, y1, x2, y2 
    if showLines5
        lowHLine5 := line.new(startTime5, low_val5, time, low_val5, xloc=xloc.bar_time, color=LineColor5, style=line.style_solid,width=linesWidth5)


else
    if inExtend5 and showLines5 and not inSession5 and timeframe.isintraday
        time1 = line.get_x1(lowHLine5)
        time2 = line.get_x2(lowHLine5)
        price = line.get_y1(lowHLine5)
        line.delete(lowHLine5)
        lowHLine5 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=LineColor5, style=line.style_solid,width=linesWidth5)


///////////////////////////////////////////////////////




//Tracer Boxe et Ligne du NY TRAP


inSession7 = not na(time(timeframe.period, rangeTime7))

startTime7 = 0
startTime7 := inSession7 and not inSession7[1] ? time : startTime7[1]

//Box lines

var line lowHLine7 = na
var line topHLine7 = na
var line leftVLine7 = na
var line rightVLine7 = na
var line middleHLine7 = na
var box bgBox7 = na

var low_val7 = 0.0
var high_val7 = 0.0
if inSession7 and not inSession7[1]
    low_val7 := low
    high_val7 := high
    high_val7

// Plot lines

if inSession7 and timeframe.isintraday
    if inSession7[1]
        line.delete(lowHLine7)
        line.delete(topHLine7)
        line.delete(leftVLine7)
        line.delete(rightVLine7)
        line.delete(middleHLine7)
        box.delete(bgBox7)

    if low < low_val7
        low_val7 := low
        low_val7
    if high > high_val7
        high_val7 := high
        high_val7

    //Create Box
    //x1, y1, x2, y2
    
    if showBackground7
        bgBox7 := box.new(startTime7, high_val7, time, low_val7, xloc=xloc.bar_time, bgcolor=backgroundColor7, border_width=0)



//////////////////////////////////////////////////


//Previous Daily High/Low

h = request.security(syminfo.tickerid, 'D', high, barmerge.gaps_off, barmerge.lookahead_on)
l = request.security(syminfo.tickerid, 'D', low, barmerge.gaps_off, barmerge.lookahead_on)




///////////////////////////////////////////////////////////////


//London Open


londonOpen(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession8 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float lnOpen = na

    if insideSession8 and not insideSession8[1]
        lnOpen := open
    else if insideSession8
        lnOpen := math.max(lnOpen, open)
    
    lnOpen
 
InSession8(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))


lnOpen  = input.session("0300-0301", title="London Open",group=GRP5)


LondonO = londonOpen(lnOpen, timeZone)


showLines8 = input(title='Show Lines', defval=true, group=GRP5)
LineColor8 = input(color.new(color.blue, 50), 'Middle Line Color', group=GRP5)
extendTime8 = input.session(title="Extend Until", defval='0300-1631', group=GRP5)
linesWidth8 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP5)


//Tracer Ligne London Open


inSession8 = not na(time(timeframe.period, lnOpen))
inExtend8 = not na(time(timeframe.period, extendTime8))

startTime8 = 0
startTime8 := inSession8 and not inSession8[1] ? time : startTime8[1]

var line lowHLine8 = na

var low_val8 = 0.0
if inSession8 and not inSession8[1]
    low_val8 := LondonO

// Plot lines

if inSession8 and timeframe.isintraday
    if inSession8[1]
        line.delete(lowHLine8)
    if low < low_val8
        low_val8 := LondonO
        low_val8


    //x1, y1, x2, y2 
    if showLines8
        lowHLine8 := line.new(startTime8, low_val8, time, low_val8, xloc=xloc.bar_time, color=LineColor8, style=line.style_solid,width=linesWidth8)


else
    if inExtend8 and showLines8 and not inSession8 and timeframe.isintraday
        time1 = line.get_x1(lowHLine8)
        time2 = line.get_x2(lowHLine8)
        price = line.get_y1(lowHLine8)
        line.delete(lowHLine8)
        lowHLine8 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=LineColor8, style=line.style_solid,width=linesWidth8)




///////////////////////////////////////////////////////////////


//NY Open


newyorkOpen(sessionTime, sessionTimeZone=syminfo.timezone) =>
    insideSession9 = not na(time(timeframe.period, sessionTime, sessionTimeZone))
    var float nyOpen = na

    if insideSession9 and not insideSession9[1]
        nyOpen := open
    else if insideSession9
        nyOpen := math.max(nyOpen, open)
    
    nyOpen
 
InSession9(sessionTimes, sessionTimeZone=syminfo.timezone) =>
    not na(time(timeframe.period, sessionTimes, sessionTimeZone))




nyOpen  = input.session("0700-0701", title="NY Open",group=GRP7)


NewyorkO = newyorkOpen(nyOpen, timeZone)


showLines9 = input(title='Show Lines', defval=true, group=GRP7)
LineColor9 = input(color.new(color.black, 50), 'Line Color', group=GRP7)
extendTime9 = input.session(title="Extend Until", defval='0700-1631', group=GRP7)
linesWidth9 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP7)
//Tracer Ligne NY Open


inSession9 = not na(time(timeframe.period, nyOpen))
inExtend9 = not na(time(timeframe.period, extendTime9))

startTime9 = 0
startTime9 := inSession9 and not inSession9[1] ? time : startTime9[1]

var line lowHLine9 = na

var low_val9 = 0.0
if inSession9 and not inSession9[1]
    low_val9 := NewyorkO

// Plot lines

if inSession9 and timeframe.isintraday
    if inSession9[1]
        line.delete(lowHLine9)
    if low < low_val9
        low_val9 := NewyorkO
        low_val9


    //x1, y1, x2, y2 
    if showLines9
        lowHLine9 := line.new(startTime9, low_val9, time, low_val9, xloc=xloc.bar_time, color=LineColor9, style=line.style_solid,width=linesWidth9)


else
    if inExtend9 and showLines9 and not inSession9 and timeframe.isintraday
        time1 = line.get_x1(lowHLine9)
        time2 = line.get_x2(lowHLine9)
        price = line.get_y1(lowHLine9)
        line.delete(lowHLine9)
        lowHLine9 := line.new(time1, price, time, price, xloc=xloc.bar_time, color=LineColor9, style=line.style_solid,width=linesWidth9)



//////////////////////////////////////////////////////////////////////

var GRP9= "Kill Zones"

st = true
londonkz = input.session(title='KillZone London Open', defval='0330-0545',group=GRP9)
newyorkkz = input.session(title='KillZone NY Open', defval='0830-1045',group=GRP9)
londonckz = input.session(title='KillZone London Close', defval='1100-1400',group=GRP9)
boxheight = input(title='Box Height', defval=3.0,group=GRP9)
ShowBoxes = input(title="Show Boxes",defval= true,group=GRP9)


dayrange = h - l
high2 = h + dayrange * 0.01 * boxheight
low2 = l - dayrange * 0.01 * boxheight

BarInSession(sess) =>
    time(timeframe.period, sess) != 0

lineColour = input(color.new(color.yellow,25), "Color LKZ")
lineColour2 = input(color.new(color.green,25), "Color NKZ")
lineColour3 = input(color.new(color.red,25),"Color LCKZ")


//LONDON
varhigh2 = plot(st and h and BarInSession(londonkz) and ShowBoxes ? high2 : na, title='Box 1 High', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varhigh = plot(st and h and BarInSession(londonkz) and ShowBoxes ? h : na, title='Box 1 Low', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varlow = plot(st and l and BarInSession(londonkz) and ShowBoxes ? l : na, title='Box 2 High', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varlow2 = plot(st and l and BarInSession(londonkz) and ShowBoxes ? low2 : na, title='Box 2 Low', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable

fill(varhigh, varhigh2, color=lineColour, title='Fill Box 1')  // box 1 top fill
fill(varlow, varlow2, color=lineColour, title='Fill Box 2')  // box 2 top fill

//NEW YORK
v1 = plot(st and h and BarInSession(newyorkkz) and ShowBoxes ? high2 : na, title='Box 3 High', style=plot.style_linebr, linewidth=2, color=na)
v2 = plot(st and h and BarInSession(newyorkkz) and ShowBoxes ? h : na, title='Box 3 Low', style=plot.style_linebr, linewidth=2, color=na)
v3 = plot(st and l and BarInSession(newyorkkz) and ShowBoxes ? l : na, title='Box 4 High', style=plot.style_linebr, linewidth=2, color=na)
v4 = plot(st and l and BarInSession(newyorkkz) and ShowBoxes ? low2 : na, title='Box 4 Low', style=plot.style_linebr, linewidth=2, color=na)

fill(v1, v2, color=lineColour2, title='Fill Box 1')
fill(v3, v4, color=lineColour2, title='Fill Box 2')


//LONDON close

varh = plot(st and h and BarInSession(londonckz) and ShowBoxes ? high2 : na, title='Box 1 High', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varh2 = plot(st and h and BarInSession(londonckz) and ShowBoxes ? h : na, title='Box 1 Low', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varl = plot(st and l and BarInSession(londonckz) and ShowBoxes ? l : na, title='Box 2 High', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable
varl2 = plot(st and l and BarInSession(londonckz) and ShowBoxes ? low2 : na, title='Box 2 Low', style=plot.style_linebr, linewidth=2, color=na)  // change color=na to color to make these lines visible/editable

fill(varh, varh2, color=lineColour3, title='Fill Box 1')  // box 1 top fill
fill(varl, varl2, color=lineColour3, title='Fill Box 2')  // box 2 top fill

///////////////////////////////////////////////


//ALGO SESSIONS

//Algos Sessions

var GRP30="Algos"

showLines30 = input(title='Show Algos', defval=true, group=GRP30)
rangeTime30 = input.session(title="Algo1", defval='0000-0131', group=GRP30)
rangeTime31 = input.session(title="Algo2", defval='0300-0431', group=GRP30)
rangeTime32 = input.session(title="Algo3", defval='0600-0731', group=GRP30)
rangeTime33 = input.session(title="Algo4", defval='0900-1031', group=GRP30)
rangeTime34 = input.session(title="Algo5", defval='1200-1331', group=GRP30)
rangeTime35 = input.session(title="Algo6", defval='1500-1631', group=GRP30)
linesWidth7 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP30)
colorAlgo = input(color.new(color.gray,50),group=GRP30)

high3 = h + dayrange * 0.5 * boxheight
low3= l - dayrange * 0.5 * boxheight
//Tracer Algo1

inSession30 = not na(time(timeframe.period, rangeTime30))

startTime30 = 0
startTime30 := inSession30 and not inSession30[1] ? time : startTime30[1]

//Box Lines
var line leftVLine30 = na
var line rightVLine30 = na

// Plot lines
if inSession30 and timeframe.isintraday
    if inSession30[1]
        line.delete(leftVLine30)
        line.delete(rightVLine30)
    

    //Create Algo
    if showLines30
        leftVLine30 := line.new(startTime30, high3, startTime30, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine30 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)

//Algo2


inSession31 = not na(time(timeframe.period, rangeTime31))

startTime31 = 0
startTime31 := inSession31 and not inSession31[1] ? time : startTime31[1]

var line leftVLine31 = na
var line rightVLine31 = na

// Plot lines
if inSession31 and timeframe.isintraday
    if inSession31[1]
        line.delete(leftVLine31)
        line.delete(rightVLine31)

    //Create Algo
    if showLines30
        leftVLine31 := line.new(startTime31, high3, startTime31, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine31 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)

//Algo3


inSession32 = not na(time(timeframe.period, rangeTime32))

startTime32 = 0
startTime32 := inSession32 and not inSession32[1] ? time : startTime32[1]

var line leftVLine32 = na
var line rightVLine32 = na

// Plot lines
if inSession32 and timeframe.isintraday
    if inSession32[1]
        line.delete(leftVLine32)
        line.delete(rightVLine32)
    //Create Algo
    if showLines30
        leftVLine32 := line.new(startTime32, high3, startTime32, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine32 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)

//Algo4


inSession33 = not na(time(timeframe.period, rangeTime33))

startTime33 = 0
startTime33 := inSession33 and not inSession33[1] ? time : startTime33[1]

var line leftVLine33 = na
var line rightVLine33 = na

// Plot lines
if inSession33 and timeframe.isintraday
    if inSession33[1]
        line.delete(leftVLine33)
        line.delete(rightVLine33)

    //Create Algo
    if showLines30
        leftVLine33 := line.new(startTime33, high3, startTime33, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine33 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)


//Algo5


inSession34 = not na(time(timeframe.period, rangeTime34))

startTime34 = 0
startTime34 := inSession34 and not inSession34[1] ? time : startTime34[1]

var line leftVLine34 = na
var line rightVLine34 = na


// Plot lines
if inSession34 and timeframe.isintraday
    if inSession34[1]
        line.delete(leftVLine34)
        line.delete(rightVLine34)
    //Create Algo
    if showLines30
        leftVLine34 := line.new(startTime34, high3, startTime34, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine34 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)

//Algo6


inSession35 = not na(time(timeframe.period, rangeTime35))

startTime35 = 0
startTime35 := inSession35 and not inSession35[1] ? time : startTime35[1]

var line leftVLine35 = na
var line rightVLine35 = na

// Plot lines
if inSession35 and timeframe.isintraday
    if inSession35[1]
        line.delete(leftVLine35)
        line.delete(rightVLine35)

    //Create Algo
    if showLines30
        leftVLine35 := line.new(startTime35, high3, startTime35, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)
        rightVLine35 := line.new(time, high3, time, low3, xloc=xloc.bar_time, color=colorAlgo, style=line.style_solid, width=linesWidth7)



////////////////////////////////////////////////////////////////////////////////////////


//Jours de La Semaine

var GRP10="Day of the Week"

user_day_toggle = input(title='Show Day Separator Lines', defval=true,group=GRP10)
mon = input(title='Monday', defval=true,group=GRP10)
tue = input(title='Tuesday', defval=true,group=GRP10)
wed = input(title='Wednesday', defval=true,group=GRP10)
thu = input(title='Thursday', defval=true,group=GRP10)
fri = input(title='Friday', defval=true,group=GRP10)
sat = input(title='Saturday', defval=false,group=GRP10)
sun = input(title='Sunday', defval=false,group=GRP10)
colorday = input(color.new(color.blue,50),title="Color Separator Lines",group=GRP10)

plotshape(mon == true and dayofweek == dayofweek.monday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Monday', text='Monday', color=color.new(color.black, 0), location=location.bottom, offset=0)
plotshape(tue == true and dayofweek == dayofweek.tuesday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Tuesday', text='Tuesday', color=color.new(color.black, 0), location=location.bottom, offset=0)
plotshape(wed == true and dayofweek == dayofweek.wednesday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Wednesday', text='Wednesday', color=color.new(color.black, 0), location=location.bottom, offset=0)
plotshape(thu == true and dayofweek == dayofweek.thursday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Thursday', text='Thursday', color=color.new(color.black, 0), location=location.bottom, offset=0)
plotshape(fri == true and dayofweek == dayofweek.friday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Friday', text='Friday', color=color.new(color.red, 0), location=location.bottom, offset=0)
plotshape(sat == true and dayofweek == dayofweek.saturday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Saturday', text='Saturday', color=color.new(color.black, 0), location=location.bottom, offset=0)
plotshape(sun == true and dayofweek == dayofweek.sunday and time(timeframe.period, '0000-0001:23456'), textcolor=color.new(color.white, 0), style=shape.labeldown, title='Sunday', text='Sunday', color=color.new(color.black, 0), location=location.bottom, offset=0)


// Get user Input

int new_day_start_time = 00

// Add the start of week line
isStartTime() =>
    hour == new_day_start_time and minute == 0 ? 1 : 0

// Add daily separator
isNewDay() =>
    dayofweek != dayofweek.sunday ? 1 : 0


bgcolor(user_day_toggle and isNewDay() == 1 and isStartTime() == 1 ? colorday : na,title='mon')
bgcolor(user_day_toggle and isNewDay() == 1 and isStartTime() == 1 ? colorday : na,title='tue')
bgcolor(user_day_toggle and isNewDay() == 1 and isStartTime() == 1 ? colorday : na,title='wed')
bgcolor(user_day_toggle and isNewDay() == 1 and isStartTime() == 1 ? colorday : na,title='thu')
bgcolor(user_day_toggle and isNewDay() == 1 and isStartTime() == 1 ? colorday : na,title='fri')
//End Section

//////////////////////////////////////////////


//PDH/L

var GRP8="PDL/H"

//--------------------------------------------------------------------
//                             Constants
//--------------------------------------------------------------------


var DAILY_LINE_STYLE    = line.style_solid
var DAILY_LINE_WITDH    = 1
var WEEKLY_LINE_STYLE   = line.style_dashed
var WEEKLY_LINE_WIDTH   = 1
var MONTHLY_LINE_STYLE  = line.style_dashed
var MONTHLY_LINE_WIDTH  = 1

//--------------------------------------------------------------------
//                               Inputs
//--------------------------------------------------------------------

g_indicator             = "Highs & Lows"
t_heads                 = "Extends previous highs and lows in the future."
t_gradient              = "Show a color gradient that highlights the recency of highs and lows."

var i_isDailyEnabled    = input     (true,          "Daily",                        inline="Daily",     group=g_indicator)
var i_dailyColor        = input     (color.green,   "",                             inline="Daily",     group=g_indicator)
var i_dailyLookback     = input.int (1,             "", 1,                          inline="Daily",     group=g_indicator)
var i_isWeeklyEnabled   = input     (false,          "Weekly",                       inline="Weekly",    group=g_indicator)
var i_weeklyColor       = input     (color.orange,  "",                             inline="Weekly",    group=g_indicator)
var i_weeklyLookback    = input.int (1,             "", 1,                          inline="Weekly",    group=g_indicator)
var i_isMonthlyEnabled  = input     (false,          "Monthly",                      inline="Monthly",   group=g_indicator)
var i_monthlyColor      = input     (color.red,     "",                             inline="Monthly",   group=g_indicator)
var i_monthlyLookback   = input.int (1,             "", 1,                          inline="Monthly",   group=g_indicator)
var i_areHeadsEnabled   = input     (true,         "Show Projections", t_heads,    inline="Head",      group=g_indicator)
var i_rightOffset       = input.int (1,            "", 1,                          inline="Head",      group=g_indicator)

//--------------------------------------------------------------------
//                        Variables declarations
//--------------------------------------------------------------------

var a_lastHighs                         = array.new_float(3)
var a_lastLows                          = array.new_float(3)
var canShowDaily                        = i_isDailyEnabled and timeframe.isintraday
var canShowWeekly                       = i_isWeeklyEnabled and (timeframe.isintraday or timeframe.isdaily)
var canShowMonthly                      = i_isMonthlyEnabled and not timeframe.ismonthly

[dailyTime, dailyHigh, dailyLow, isLastDaily]           = request.security(syminfo.tickerid, 'D', [time, high, low, barstate.islast], lookahead=barmerge.lookahead_on)
[weeklyTime, weeklyHigh, weeklyLow, isLastWeekly]       = request.security(syminfo.tickerid, 'W', [time, high, low, barstate.islast], lookahead=barmerge.lookahead_on)
[monthlyTime, monthlyHigh, monthlyLow, isLastMonthly]   = request.security(syminfo.tickerid, 'M', [time, high, low, barstate.islast], lookahead=barmerge.lookahead_on)

hasDailyTimeChanged                     = dailyTime != dailyTime[1]
hasWeekklyTimeChanged                   = weeklyTime != weeklyTime[1]
hasMonthlyTimeChanged                   = monthlyTime != monthlyTime[1]

//--------------------------------------------------------------------
//                              Functions 
//--------------------------------------------------------------------

f_getRightBarIndex() => bar_index + i_rightOffset

f_draw(bool _isNew, float _h, float _l, int _lookback, color _color, string _style, int _width) =>
    var line _high  = na
    var line _low   = na
    var _highs      = array.new_line()
    var _lows       = array.new_line()
    _end            = i_areHeadsEnabled ? f_getRightBarIndex() : bar_index
    
    if _isNew
        line.set_x2(_high, _end)
        line.set_x2(_low, _end)


        _high := line.new(bar_index, _h, bar_index, _h, color=_color, style=_style, width=_width)
        _low := line.new(bar_index, _l, bar_index, _l, color=_color, style=_style, width=_width)
        array.push(_highs, _high)
        array.push(_lows, _low)

        if array.size(_highs) > _lookback + 1
            line.delete(array.shift(_highs))
            line.delete(array.shift(_lows))

	if i_areHeadsEnabled and barstate.islast and array.size(_highs) > 1
		// Avoid updating the last/current high and low 
		for i = 0 to array.size(_highs) - 2
			line.set_x2(array.get(_highs, i), f_getRightBarIndex())
			line.set_x2(array.get(_lows, i), f_getRightBarIndex())

//--------------------------------------------------------------------
//                                Logic
//--------------------------------------------------------------------

if canShowDaily and hasDailyTimeChanged and not isLastDaily
	array.set(a_lastHighs, 0, dailyHigh)
	array.set(a_lastLows, 0, dailyLow)

if canShowWeekly and hasWeekklyTimeChanged and not isLastWeekly
	array.set(a_lastHighs, 1, weeklyHigh)
	array.set(a_lastLows, 1, weeklyLow)

if canShowMonthly and hasMonthlyTimeChanged and not isLastMonthly
	array.set(a_lastHighs, 2, monthlyHigh)
	array.set(a_lastLows, 2, monthlyLow)

//--------------------------------------------------------------------
//                          Plotting & styling
//--------------------------------------------------------------------

if canShowMonthly
    f_draw(hasMonthlyTimeChanged, monthlyHigh, monthlyLow, i_monthlyLookback, i_monthlyColor, MONTHLY_LINE_STYLE, MONTHLY_LINE_WIDTH)

if canShowWeekly
    f_draw(hasWeekklyTimeChanged, weeklyHigh, weeklyLow, i_weeklyLookback, i_weeklyColor, WEEKLY_LINE_STYLE, WEEKLY_LINE_WIDTH)

if canShowDaily
    f_draw(hasDailyTimeChanged, dailyHigh, dailyLow, i_dailyLookback, i_dailyColor, DAILY_LINE_STYLE, DAILY_LINE_WITDH)

// Plot invisible highs and lows for displaying their last values in `status line`, `scale`, `data window` as well for providing defaults alert conditions

plot(array.get(a_lastHighs, 0), "DH",   color.new(i_dailyColor, 100),   editable=false)
plot(array.get(a_lastLows, 0),  "DL",   color.new(i_dailyColor, 100),   editable=false)
plot(array.get(a_lastHighs, 1), "WH",   color.new(i_weeklyColor, 100),  editable=false)
plot(array.get(a_lastLows, 1),  "WL",   color.new(i_weeklyColor, 100),  editable=false)
plot(array.get(a_lastHighs, 2), "MH",   color.new(i_monthlyColor, 100), editable=false)
plot(array.get(a_lastLows, 2),  "ML",   color.new(i_monthlyColor, 100), editable=false)




////////////////////////////////////////////

//Imbalance


showImbalance = input(true,title="Show Imbalances",group="Imbalance")
imbRestClr = input.color(#ffeb3b, title="Color Imbalance",group="Imbalance")
var float topValue = na, var float bottomValue = na

var line topLine = na, var line bottomLine = na
var box demandBox = na, var box supplyBox = na

var box imbalanceBox = na

//IMBALANCE
//Data
L1 = low
H3 = high[2]

H1 = high
L3 = low[2]


FVGUp = H3 < L1 ? 1 : 0
plotFVGU = FVGUp ? H3 : na
plotFVGUL = FVGUp ? L1 : na

FVGDown = L3 > H1 ? 1 : 0
plotFVGD = FVGDown ? L3 : na
plotFVGH = FVGDown ? H1 : na

barcolor(FVGDown and showImbalance ? imbRestClr : na,offset = -1)
barcolor(FVGUp and showImbalance ? imbRestClr : na,offset = -1)

/////////////////////////////////////////////////////////////


//CLS Zones

var GRP11="CLS Zone"

showLines11 = input(title='Show CLS Zones', defval=true, group=GRP11)
rangeTime11 = input.session(title="CLS Asian", defval='0000-0101', group=GRP11)
rangeTime12 = input.session(title="CLS Euro", defval='0300-0401', group=GRP11)
rangeTime13 = input.session(title="CLS NY", defval='0600-0701', group=GRP11)
rangeTime14 = input.session(title="CLS Other", defval='0900-1001', group=GRP11)
colorClsLines = input(color.new(color.red,20),title="Color CLS Lines",group=GRP11)
linesWidth11 = input.int(1, 'Lines Width', minval=1, maxval=4, group=GRP11)

high4 = h + dayrange * 0.5 * boxheight
low4 = l - dayrange * 0.5 * boxheight


//Tracer CLS Asian

inSession11 = not na(time(timeframe.period, rangeTime11))

startTime11 = 0
startTime11 := inSession11 and not inSession11[1] ? time : startTime11[1]

//Box Lines
var line leftVLine11 = na
var line rightVLine11 = na


// Plot lines
if inSession11 and timeframe.isintraday
    if inSession11[1]
        line.delete(leftVLine11)
        line.delete(rightVLine11)
    
    //Create CLS
    if showLines11
        leftVLine11 := line.new(startTime11, high4, startTime11, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)
        rightVLine11 := line.new(time, high4, time, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)

//CLS London


inSession12 = not na(time(timeframe.period, rangeTime12))

startTime12 = 0
startTime12 := inSession12 and not inSession12[1] ? time : startTime12[1]

var line leftVLine12 = na
var line rightVLine12 = na

// Plot lines
if inSession12 and timeframe.isintraday
    if inSession12[1]
        line.delete(leftVLine12)
        line.delete(rightVLine12)

    //Create CLS
    if showLines11
        leftVLine12 := line.new(startTime12, high4, startTime12, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)
        rightVLine12 := line.new(time, high4, time, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)

// CLS NY


inSession13 = not na(time(timeframe.period, rangeTime13))

startTime13 = 0
startTime13 := inSession13 and not inSession13[1] ? time : startTime13[1]

var line leftVLine13 = na
var line rightVLine13 = na



// Plot lines
if inSession13 and timeframe.isintraday
    if inSession13[1]
        line.delete(leftVLine13)
        line.delete(rightVLine13)


    //Create CLS
    if showLines11
        leftVLine13 := line.new(startTime13, high4, startTime13, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)
        rightVLine13 := line.new(time, high4, time, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)

// CLS Last


inSession14 = not na(time(timeframe.period, rangeTime14))

startTime14 = 0
startTime14 := inSession14 and not inSession14[1] ? time : startTime14[1]

var line leftVLine14 = na
var line rightVLine14 = na


// Plot lines
if inSession14 and timeframe.isintraday
    if inSession14[1]
        line.delete(leftVLine14)
        line.delete(rightVLine14)


    //Create CLS
    if showLines11
        leftVLine14 := line.new(startTime14, high4, startTime14, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)
        rightVLine14 := line.new(time, high4, time, low4, xloc=xloc.bar_time, color=colorClsLines, style=line.style_solid, width=linesWidth11)



//////////////////////////////////////////////

// Fractals


var GRP0="Fractal"
fractalShow = input(title="Show Fractal",defval=true,group=GRP0)
fractalPeriods = input.int(title='Fractal Periods', defval=2, minval=1,group=GRP0)
colorBullFractal = input(color.new(color.red,0),title="Color Bear Fractal",group=GRP0)
colorBearFractal = input(color.new(color.green,0),title="Color Bull Fractal",group=GRP0)
bullFractal = ta.pivothigh(fractalPeriods, fractalPeriods)
bearFractal = ta.pivotlow(fractalPeriods, fractalPeriods)
plotshape(bullFractal and fractalShow ? true : na, title='Bull Fractal',style=shape.triangledown, location=location.abovebar, offset=-fractalPeriods, color=colorBullFractal)
plotshape(bearFractal and fractalShow? true : na, title='Bear Fractal',style=shape.triangleup, location=location.belowbar, offset=-fractalPeriods, color=colorBearFractal)


////////////////////////////////////////////////////////

//Notes

var table NotesTable = table.new(tabinp2, 2, 20, bgcolor=TableBG2, border_width=1)
if barstate.islast and Table2
	Cell = 0
	Cell += 1
	//Notes
	Cell += 1
	table.cell(NotesTable, 0, Cell, text = notes, text_size = sizeText , text_color = Tab2txtCol, text_halign=text.align_left)
	table.merge_cells(NotesTable, 0, Cell, 1, Cell)
