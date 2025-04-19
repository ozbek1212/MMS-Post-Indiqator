// This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © TFlab

//@version=5
indicator("Market Structures SMC [TradingFinder] BOS/CHoCH Major & Minor" ,'Market Structure TFlab', overlay = true, max_bars_back = 5000, max_boxes_count = 500,max_labels_count = 500, max_lines_count = 500 )

//Input
    //Pivot Period of Order Blocks Detector
PP = input.int(5, 'Pivot Period of Order Blocks Detector' , group = 'Logic Parameter' , minval = 1)

    //Lines

        //Bos Lines
            //Major Line
                //Bullish
MajorBuBoSLine_Show       = input.string('On'  ,'Show Major Bullish BoS Lines', ['On', 'Off'] , group = 'Major Bullish "BoS" Lines')
MajorBuBoSLine_Style    = input.string(line.style_solid ,'Style Major Bullish BoS Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted]  , group = 'Major Bullish "BoS" Lines')
MajorBuBoSLine_Color    = input.color(color.rgb(1, 170, 221) ,'Color Major Bullish BoS Lines'  , group = 'Major Bullish "BoS" Lines')


                //Bearish
MajorBeBoSLine_Show       = input.string('On'  ,'Show Major Bearish BoS Lines', ['On', 'Off'] , group = 'Major Bearish "BoS" Lines')
MajorBeBoSLine_Style    = input.string(line.style_solid ,'Style Major Bearish BoS Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted]  , group = 'Major Bearish "BoS" Lines')
MajorBeBoSLine_Color    = input.color(color.rgb(239, 188, 101) ,'Color Major Bearish BoS Lines'  , group = 'Major Bearish "BoS" Lines')


            //Minor
                //Bullish
MinorBuBoSLine_Show       = input.string('On'  ,'Show Minor Bullish BoS  Lines', ['On', 'Off'] , group = 'Minor Bullish "BoS"  Lines')
MinorBuBoSLine_Style    = input.string(line.style_dashed ,'Style Minor Bullish BoS  Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted]  , group = 'Minor Bullish "BoS"  Lines')
MinorBuBoSLine_Color    = input.color(color.black ,'Color Minor Bullish BoS  Lines' , group = 'Minor Bullish "BoS"  Lines' )


                //Bearish
MinorBeBoSLine_Show       = input.string('On'  ,'Show Minor Bearish BoS Lines', ['On', 'Off'] , group = 'Minor Bearish "BoS" Lines' )
MinorBeBoSLine_Style    = input.string(line.style_dashed ,'Style inor Bearish BoS Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted] , group = 'Minor Bearish "BoS" Lines')
MinorBeBoSLine_Color    = input.color(color.black ,'Color inor Bearish BoS Lines' , group = 'Minor Bearish "BoS" Lines')

        //ChoCh Lines
            //Major Line
                //Bullish
MajorBuChoChLine_Show     = input.string('On'  ,'Show Major Bullish ChoCh Lines', ['On', 'Off'] , group = 'Major Bullish "ChoCh" Lines')
MajorBuChoChLine_Style    = input.string(line.style_solid ,'Style Major Bullish ChoCh Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted] , group = 'Major Bullish "ChoCh" Lines')
MajorBuChoChLine_Color    = input.color(color.rgb(2, 124, 94) ,'Color Major Bullish ChoCh Lines' , group = 'Major Bullish "ChoCh" Lines')

                //Bearish
MajorBeChoChLine_Show     = input.string('On'  ,'Show Major Bearish ChoCh Lines', ['On', 'Off'] , group = 'Major Bearish "ChoCh" Lines')
MajorBeChoChLine_Style    = input.string(line.style_solid ,'Style Major Bearish ChoCh Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted]  , group = 'Major Bearish "ChoCh" Lines')
MajorBeChoChLine_Color    = input.color(color.rgb(142, 32, 67) ,'Color Major Bearish ChoCh Lines'  , group = 'Major Bearish "ChoCh" Lines')

            //Minor
                //Bullish
MinorBuChoChLine_Show     = input.string('On'  ,'Show Minor Bullish ChoCh Lines', ['On', 'Off'] , group = 'Minor Bullish "ChoCh" Lines')
MinorBuChoChLine_Style    = input.string(line.style_dashed ,'Style Minor Bullish ChoCh Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted] , group = 'Minor Bullish "ChoCh" Lines')
MinorBuChoChLine_Color    = input.color(color.black ,'Color Minor Bullish ChoCh Lines' , group = 'Minor Bullish "ChoCh" Lines')

                //Bearish           
MinorBeChoChLine_Show     = input.string('On'  ,'Show Minor Bearish ChoCh Lines', ['On', 'Off'] , group = 'Minor Bearish "ChoCh" Lines')
MinorBeChoChLine_Style    = input.string(line.style_dashed ,'Style Minor Bearish ChoCh Lines', 
 [line.style_solid, line.style_dashed, line.style_dotted]  , group = 'Minor Bearish "ChoCh" Lines')
MinorBeChoChLine_Color    = input.color(color.black ,'Color Minor Bearish ChoCh Lines'  , group = 'Minor Bearish "ChoCh" Lines')

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


//Variables

    //ZigZag Data
Open = open
High = high
Low = low
Close = close 
Bar_Index = bar_index
ATR =ta.atr(55)
var ArrayType = array.new_string()
var ArrayValue = array.new_float()
var ArrayIndex =  array.new_int()
var ArrayTypeAdv  = array.new_string()
var ArrayValueAdv = array.new_float()
var ArrayIndexAdv =  array.new_int()  
var line ZZLine = na
var line Sline = na 
var line Rline = na 
var label Label  = na
PASS = 0
HighPivot = ta.pivothigh(PP,PP)
LowPivot = ta.pivotlow(PP,PP)
HighValue = ta.valuewhen(HighPivot ,High[PP], 0)
LowValue = ta.valuewhen(LowPivot ,Low[PP], 0)
HighIndex = ta.valuewhen(HighPivot ,Bar_Index[PP], 0)
LowIndex = ta.valuewhen(LowPivot ,Bar_Index[PP], 0)
Correct_HighPivot = 0.0
Correct_LowPivot =  0.0

    //Major Levels
var float Major_HighLevel = na
var float Major_LowLevel  = na

var int Major_HighIndex = na
var int Major_LowIndex = na

var string Major_HighType= na
var string Major_LowType = na

    //Minor Levels
var float Minor_HighLevel = na
var float Minor_LowLevel  = na

var int Minor_HighIndex   = na
var int Minor_LowIndex    = na

var string Minor_HighType = na
var string Minor_LowType  = na

var int LockDetecteM_MinorLvL = 0

    //
var bool Lock0 = true
var bool Lock1 = true

// Detecte Major & Minor Pivot
// Order Blocks Data
    //Major
var int LastMHH = 0
var int Last02MHH = 0
var int LastMLH = 0

var int LastMLL = 0
var int Last02MLL = 0
var int LastMHL = 0
    //Minor
var int LastmHH = 0
var int Last02mHH = 0
var int LastmLH = 0

var int LastmLL = 0
var int Last02mLL = 0
var int LastmHL = 0

/////////////////////////////
// Last Pivot First Point////
/////////////////////////////
var string LastPivotType = na
var int LastPivotIndex = 0

var string LastPivotType02 = na
var int LastPivotIndex02 = 0

    //Major 01
var float MajorHighValue01 = na
var int   MajorHighIndex01 = na
var string MajorHighType01 = ''

var float MajorLowValue01  = na
var int   MajorLowIndex01 =  na
var string MajorLowType01 =  ''

    //Minor 01
var float MinorHighValue01 = na
var int   MinorHighIndex01 = na
var string MinorHighType01 = ''

var float MinorLowValue01  = na
var int   MinorLowIndex01  = na
var string MinorLowType01 =  ''

/////////////////////////////////////
//One to the Last Pivot First Point//
/////////////////////////////////////
    //Major 02
var float MajorHighValue02 = na
var int   MajorHighIndex02 = na
var string MajorHighType02 = ''

var float MajorLowValue02  = na
var int   MajorLowIndex02 =  na
var string MajorLowType02 =  ''

    //Minor 02
var float MinorHighValue02 = na
var int   MinorHighIndex02 = na
var string MinorHighType02 = ''

var float MinorLowValue02  = na
var int   MinorLowIndex02  = na
var string MinorLowType02 =  ''

    //Major 02 Change Type Pivot
var float MajorHighValue02Ch = na
var int   MajorHighIndex02Ch = na
var string MajorHighType02Ch = ''

var float MajorLowValue02Ch  = na
var int   MajorLowIndex02Ch =  na
var string MajorLowType02Ch =  ''

    //Lines Data
var line MajorLine_ChoChBull   = na
var label MajorLabel_ChoChBull = na

var line MajorLine_ChoChBear   = na
var label MajorLabel_ChoChBear = na

var line MajorLine_BoSBull     = na
var label MajorLabel_BoSBull   = na

var line MajorLine_BoSBear     = na
var label MajorLabel_BoSBear   = na

var line MinorLine_ChoChBull   = na
var label MinorLabel_ChoChBull = na

var line MinorLine_ChoChBear   = na
var label MinorLabel_ChoChBear = na

var line MinorLine_BoSBull     = na
var label MinorLabel_BoSBull   = na

var line MinorLine_BoSBear     = na
var label MinorLabel_BoSBear   = na

    // BoS & ChoCh Data
        //Major
var bool Bullish_Major_ChoCh = false
var bool Bullish_Major_BoS = false

var bool Bearish_Major_ChoCh = false
var bool Bearish_Major_BoS = false

var BoS_MajorType  = array.new_string()
var BoS_MajorIndex = array.new_int()

var ChoCh_MajorType  = array.new_string()
var ChoCh_MajorIndex = array.new_int()

var int LockBreak_M = 0 

        //Minor
var bool Bullish_Minor_ChoCh = false
var bool Bullish_Minor_BoS = false

var bool Bearish_Minor_ChoCh = false
var bool Bearish_Minor_BoS = false

var BoS_MinorType  = array.new_string()
var BoS_MinorIndex = array.new_int()

var ChoCh_MinorType  = array.new_string()
var ChoCh_MinorIndex = array.new_int()

var int LockBreak_m = 0 

    //Trend Data

var string ExternalTrend = 'No Trend'
var string InternalTrend = 'No Trend'


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////Calculation/////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

    //{Zig Zag}
if HighPivot  and  LowPivot
    if ArrayType.size() == 0
        PASS := 1
    else if ArrayType.size() >= 1
        if ((ArrayType.get(ArrayType.size() - 1)) == "L" or
             (ArrayType.get(ArrayType.size() - 1)) == "LL")
            if LowPivot < ArrayValue.get(ArrayType.size() - 1)
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1) 
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
                Correct_LowPivot :=  LowValue
            else 
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H" ) ///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
            Correct_HighPivot := HighValue  
        else if (ArrayType.get(ArrayType.size() - 1)) == "H" or 
             (ArrayType.get(ArrayType.size() - 1)) == "HH"
            if HighPivot > ArrayValue.get(ArrayType.size() - 1)
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1)
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H")///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
                Correct_HighPivot := HighValue  
            else 
                array.push(ArrayType,ArrayType.size()>2?  ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
            Correct_LowPivot :=  LowValue    
        else if (ArrayType.get(ArrayType.size() - 1)) == "LH"
            if HighPivot < ArrayValue.get(ArrayType.size() - 1)
                array.push(ArrayType,ArrayType.size()>2?  ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
                Correct_LowPivot :=  LowValue 
            else if HighPivot > ArrayValue.get(ArrayType.size() - 1)
                if close < ArrayValue.get(ArrayType.size() - 1)
                    array.remove(ArrayType,ArrayType.size() - 1)
                    array.remove(ArrayValue,ArrayValue.size() - 1)
                    array.remove(ArrayIndex,ArrayIndex.size() - 1)
                    array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H")///////////////////////////////Here
                    array.push(ArrayValue, HighValue)
                    array.push(ArrayIndex, HighIndex)
                    Correct_HighPivot := HighValue  
                else if close > ArrayValue.get(ArrayType.size() - 1)
                    array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                    array.push(ArrayValue, LowValue)
                    array.push(ArrayIndex, LowIndex)
                    Correct_LowPivot :=  LowValue
        else if (ArrayType.get(ArrayType.size() - 1)) == "HL"
            if LowPivot > ArrayValue.get(ArrayType.size() - 1)
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H" ) ///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
                Correct_HighPivot := HighValue                       
            else if LowPivot < ArrayValue.get(ArrayType.size() - 1)
                if close > ArrayValue.get(ArrayType.size() - 1)
                    array.remove(ArrayType,ArrayType.size() - 1)
                    array.remove(ArrayValue,ArrayValue.size() - 1)
                    array.remove(ArrayIndex,ArrayIndex.size() - 1) 
                    array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                    array.push(ArrayValue, LowValue)
                    array.push(ArrayIndex, LowIndex)
                    Correct_LowPivot :=  LowValue
                else if close < ArrayValue.get(ArrayType.size() - 1)
                    array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H")///////////////////////////////Here
                    array.push(ArrayValue, HighValue)
                    array.push(ArrayIndex, HighIndex)
                    Correct_HighPivot := HighValue                         
else if  HighPivot 
    if ArrayType.size() == 0
        array.insert(ArrayType, 0, "H")
        array.insert(ArrayValue, 0, HighValue)
        array.insert(ArrayIndex, 0, HighIndex)
        Correct_HighPivot := HighValue
    else if ArrayType.size() >= 1
        if ((ArrayType.get(ArrayType.size() - 1)) == "L" or
             (ArrayType.get(ArrayType.size() - 1)) == "HL" or
             (ArrayType.get(ArrayType.size() - 1)) == "LL")
            if HighPivot > ArrayValue.get(ArrayType.size() - 1)
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H" ) ///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
                Correct_HighPivot := HighValue
            else if  HighPivot < ArrayValue.get(ArrayType.size() - 1)
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1) 
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
                Correct_LowPivot :=  LowValue                         
        else if (ArrayType.get(ArrayType.size() - 1)) == "H" or 
             (ArrayType.get(ArrayType.size() - 1)) == "HH" or 
             (ArrayType.get(ArrayType.size() - 1)) == "LH"
            if (ArrayValue.get(ArrayValue.size() - 1)) < HighValue
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1)
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H")///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
                Correct_HighPivot := HighValue               
else if LowPivot 
    if ArrayType.size() == 0
        array.insert(ArrayType, 0, "L")
        array.insert(ArrayValue, 0, LowValue)
        array.insert(ArrayIndex, 0, LowIndex)
        Correct_LowPivot :=  LowValue
    else if ArrayType.size() >= 1
        if (ArrayType.get(ArrayType.size() - 1)) == "H" or 
             (ArrayType.get(ArrayType.size() - 1)) == "HH" or 
             (ArrayType.get(ArrayType.size() - 1)) == "LH"
            if LowPivot < ArrayValue.get(ArrayType.size() - 1)
                array.push(ArrayType,ArrayType.size()>2?  ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
                Correct_LowPivot :=  LowValue
            else if LowPivot > ArrayValue.get(ArrayType.size() - 1)
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1)
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < HighValue ? "HH" : "LH" : "H")///////////////////////////////Here
                array.push(ArrayValue, HighValue)
                array.push(ArrayIndex, HighIndex)
                Correct_HighPivot := HighValue                        
        else if (ArrayType.get(ArrayType.size() - 1)) == "L" or 
             (ArrayType.get(ArrayType.size() - 1)) == "HL" or 
             (ArrayType.get(ArrayType.size() - 1)) == "LL"
            if (ArrayValue.get(ArrayValue.size() - 1)) > LowValue
                array.remove(ArrayType,ArrayType.size() - 1)
                array.remove(ArrayValue,ArrayValue.size() - 1)
                array.remove(ArrayIndex,ArrayIndex.size() - 1) 
                array.push(ArrayType,ArrayType.size()>2? ArrayValue.get(ArrayType.size() - 2) < LowValue ? "HL" : "LL" : "L")///////////////////////////////Here
                array.push(ArrayValue, LowValue)
                array.push(ArrayIndex, LowIndex)
                Correct_LowPivot :=  LowValue

    //{Zig Zag Advance}

        //first Major & Minor Detector
if ArrayType.size() ==  2
    if ArrayType.get(0) == 'H'
        Major_HighLevel := ArrayValue.get(0)
        Major_LowLevel  := ArrayValue.get(1)
        Major_HighIndex := ArrayIndex.get(0)
        Major_LowIndex  := ArrayIndex.get(1)
        Major_HighType := ArrayType.get(0)
        Major_LowType  := ArrayType.get(1)
    else if ArrayType.get(0) == 'L'
        Major_HighLevel := ArrayValue.get(1)
        Major_LowLevel  := ArrayValue.get(0) 
        Major_HighIndex := ArrayIndex.get(1)
        Major_LowIndex  := ArrayIndex.get(0)
        Major_HighType := ArrayType.get(1)
        Major_LowType  := ArrayType.get(0)

        //Making Copies of Arrays


if  ArrayValue.size() == 1
    if Lock0
        array.insert(ArrayTypeAdv ,0 ,'M' +  ArrayType.get(0))
        array.insert(ArrayValueAdv,0 ,ArrayValue.get(0))
        array.insert(ArrayIndexAdv,0 ,ArrayIndex.get(0))   
        Lock0 := false

if  ArrayValue.size() == 2
    if Lock1
        array.insert(ArrayTypeAdv ,1 ,'M' +  ArrayType.get(1))
        array.insert(ArrayValueAdv,1 ,ArrayValue.get(1))
        array.insert(ArrayIndexAdv,1 ,ArrayIndex.get(1))   
        Lock1 := false

if ArrayValue.size() > 1
    if ArrayValue.get(ArrayValue.size()-1)[1] != ArrayValue.get(ArrayValue.size()-1)
        if str.substring(ArrayType.get(ArrayType.size()-1)[1], str.length(ArrayType.get(ArrayType.size()-1))-1) != 
             str.substring(ArrayType.get(ArrayType.size()-1), str.length(ArrayType.get(ArrayType.size()-1))-1)
            array.push(ArrayTypeAdv ,'m' +  ArrayType.get(ArrayType.size()   - 1))
            array.push(ArrayValueAdv, ArrayValue.get(ArrayValue.size() - 1))
            array.push(ArrayIndexAdv, ArrayIndex.get(ArrayIndex.size() - 1))
        else if str.substring(ArrayType.get(ArrayType.size()-1)[1], str.length(ArrayType.get(ArrayType.size()-1))-1) == 
             str.substring(ArrayType.get(ArrayType.size()-1), str.length(ArrayType.get(ArrayType.size()-1))-1)
            array.remove(ArrayValueAdv, ArrayValueAdv.size() - 1)
            array.remove(ArrayIndexAdv, ArrayIndexAdv.size() - 1)
            array.push(ArrayValueAdv, ArrayValue.get(ArrayValue.size() - 1))
            array.push(ArrayIndexAdv, ArrayIndex.get(ArrayIndex.size() - 1))

        //All Major & Minor Pivot Detector 

if ArrayValueAdv.size() > 1
            //High Major Detector
    if close > Major_HighLevel
        if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('ML')
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1))      
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) 
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M'  + ArrayType.get(ArrayType.size() - 1)) 
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH' or 
             ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MLH' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MHH' 
            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mHL'
                ArrayTypeAdv.remove(ArrayTypeAdv.size() - 2)
                ArrayTypeAdv.insert(ArrayValueAdv.size() - 2 , 'M' + ArrayType.get(ArrayType.size() - 2))      
                Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mLL'
                ArrayTypeAdv.remove(ArrayTypeAdv.size() - 2)
                ArrayTypeAdv.insert(ArrayValueAdv.size() - 2 , 'M' + ArrayType.get(ArrayType.size() - 2))
                Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayValueAdv.get(ArrayValueAdv.size() - 1) > Major_HighLevel
        if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('MH')
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1))  
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)     
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MHH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1))
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)

            //Low Major Detector
    if close < Major_LowLevel
        if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('MH')
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' +  ArrayType.get(ArrayType.size() - 1))      
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1))
            Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL' or 
             ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MHL' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MLL'
            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mLH'
                ArrayTypeAdv.remove(ArrayTypeAdv.size() - 2)
                ArrayTypeAdv.insert(ArrayValueAdv.size() - 2 , 'M' + ArrayType.get(ArrayType.size() - 2))      
                Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mHH'
                ArrayTypeAdv.remove(ArrayTypeAdv.size() - 2)
                ArrayTypeAdv.insert(ArrayValueAdv.size() - 2 , 'M' + ArrayType.get(ArrayType.size() - 2))
                Major_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Major_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Major_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if ArrayValueAdv.get(ArrayValueAdv.size() - 1) < Major_LowLevel
        if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('ML')
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)

        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1) ) 
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'MLL'
            ArrayTypeAdv.remove(ArrayTypeAdv.size() - 1)
            ArrayTypeAdv.push('M' + ArrayType.get(ArrayType.size() - 1))
            Major_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
            Major_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
            Major_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)



//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


    //Get Pivot Data
        //Last Pivot Data
if ArrayTypeAdv.size() > 1
    LastPivotType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
    LastPivotIndex := ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    LastPivotType02 := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
    LastPivotIndex02 := ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        //Major
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'MHH'
        MajorHighValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MajorHighIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MajorHighType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastMHH            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'MLH'
        MajorHighValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MajorHighIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MajorHighType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastMLH            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'MLL'
        MajorLowValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MajorLowIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MajorLowType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastMLL            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'MHL'
        MajorLowValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MajorLowIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MajorLowType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastMHL            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    
        //Minor
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'mHH'
        MinorHighValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MinorHighIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MinorHighType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastmHH            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'mLH'
        MinorHighValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MinorHighIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MinorHighType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastmLH            :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'mLL'
        MinorLowValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MinorLowIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MinorLowType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastmLL           :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) ==  'mHL'
        MinorLowValue01   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
        MinorLowIndex01   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
        MinorLowType01    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
        LastmHL           :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)

    //One to the Last Pivot Data
if ArrayTypeAdv.size() > 1
        //Major
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MHH'
        MajorHighValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MajorHighIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MajorHighType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MLH'
        MajorHighValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MajorHighIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MajorHighType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MLL'
        MajorLowValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MajorLowIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MajorLowType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MHL'
        MajorLowValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MajorLowIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MajorLowType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    
        //Minor
    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'mHH'
        MinorHighValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MinorHighIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MinorHighType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'mLH'
        MinorHighValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MinorHighIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MinorHighType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'mLL'
        MinorLowValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MinorLowIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MinorLowType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'mHL'
        MinorLowValue02   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
        MinorLowIndex02   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
        MinorLowType02    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        // Change Pivot Type to Major
    if ArrayTypeAdv.get(ArrayTypeAdv.size()-2) != ArrayTypeAdv.get(ArrayTypeAdv.size()-2)[1] // Change Pivot Type to Major
        //Major
        if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MHH'
            MajorHighValue02Ch   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            MajorHighIndex02Ch   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            MajorHighType02Ch    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MLH'
            MajorHighValue02Ch   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            MajorHighIndex02Ch   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            MajorHighType02Ch    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MLL'
            MajorLowValue02Ch   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            MajorLowIndex02Ch   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            MajorLowType02Ch    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        if  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) ==  'MHL'
            MajorLowValue02Ch   :=  ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            MajorLowIndex02Ch   :=  ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            MajorLowType02Ch    :=  ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    // Detecte Minor level

if ArrayTypeAdv.size() > 2  
    LockDetecteM_MinorLvL := 1
    if str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 1),'m') == 0 and
     str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 2),'m') == 0   and
     str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 3),'M') == 0
        if str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 1),'H') == 2
            Minor_HighLevel := ArrayValueAdv.get(ArrayTypeAdv.size() - 1)
            Minor_LowLevel  := ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            Minor_HighIndex := ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
            Minor_LowIndex  := ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            Minor_HighType  := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
            Minor_LowType   := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
        else if str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 1),'L') == 2
            Minor_HighLevel := ArrayValueAdv.get(ArrayTypeAdv.size() - 2)
            Minor_LowLevel  := ArrayValueAdv.get(ArrayTypeAdv.size() - 1) 
            Minor_HighIndex := ArrayIndexAdv.get(ArrayTypeAdv.size() - 2)
            Minor_LowIndex  := ArrayIndexAdv.get(ArrayTypeAdv.size() - 1)
            Minor_HighType  := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
            Minor_LowType   := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
    if LockDetecteM_MinorLvL == 1
        //High Minor Detector
        if close > Minor_HighLevel

            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL'     
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) 
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL'
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH'
                if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mHL'     
                    Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                    Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                    Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
                else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mLL'
                    Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                    Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                    Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
                    

        if  ArrayValueAdv.get(ArrayValueAdv.size() - 1) > Minor_HighLevel

            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH' 
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) 
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)                    
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH'
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        //Low Minor Detector
        if close < Minor_LowLevel

            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLH'     
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHH'
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL' or ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL'
                if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mLH'     
                    Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                    Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                    Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
                else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 2) == 'mHH'
                    Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                    Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                    Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

        if ArrayValueAdv.get(ArrayValueAdv.size() - 1) < Minor_LowLevel
            if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mHL' 
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)
            else if ArrayTypeAdv.get(ArrayTypeAdv.size() - 1) == 'mLL' 
                Minor_LowLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 1)
                Minor_LowIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 1)
                Minor_LowType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 1)
                Minor_HighLevel := ArrayValueAdv.get(ArrayValueAdv.size() - 2)
                Minor_HighIndex := ArrayIndexAdv.get(ArrayIndexAdv.size() - 2)
                Minor_HighType := ArrayTypeAdv.get(ArrayTypeAdv.size() - 2)

    //Reset Minor Levels
    if str.pos(ArrayTypeAdv.get(ArrayTypeAdv.size() - 1),'M') == 0
        LockDetecteM_MinorLvL := 0
        Minor_HighLevel := na
        Minor_LowLevel  := na
        Minor_HighIndex := na
        Minor_LowIndex  := na
        Minor_HighType  := na
        Minor_LowType   := na
        InternalTrend   := 'No Trend'


    //ChoCh and BoS Detector
        //Bos and ChoCh Line
            //Major

if  ta.crossover(close , Major_HighLevel) and  LockBreak_M != Major_HighIndex // Bullish BoS Detector
    if (ExternalTrend == 'No Trend' or ExternalTrend == 'Up Trend')
        Bullish_Major_BoS := true
        BoS_MajorType.push('Bull Major BoS')
        BoS_MajorIndex.push(bar_index)
        LockBreak_M := Major_HighIndex
        ExternalTrend := 'Up Trend'
        if MajorBuBoSLine_Show == 'On'
            MajorLine_BoSBull     := line.new(Major_HighIndex, Major_HighLevel , bar_index , Major_HighLevel , style = MajorBuBoSLine_Style , color = MajorBuBoSLine_Color)
            MajorLabel_BoSBull    := label.new((Major_HighIndex + bar_index) / 2 , Major_HighLevel   , 
             text = 'Major BoS' , color = color.rgb(0,0,0,100), textcolor = MajorBuBoSLine_Color ,size = size.normal )
    else if ExternalTrend == 'Down Trend' // Bullish ChoCh Detector
        Bullish_Major_ChoCh := true
        ChoCh_MajorType.push('Bull Major ChoCh')
        ChoCh_MajorIndex.push(bar_index)
        LockBreak_M := Major_HighIndex
        ExternalTrend := 'Up Trend'
        if MajorBuChoChLine_Show == 'On'
            MajorLine_ChoChBull    := line.new(Major_HighIndex, Major_HighLevel , bar_index , Major_HighLevel , style = MajorBuChoChLine_Style , color = MajorBuChoChLine_Color)
            MajorLabel_ChoChBull   := label.new((Major_HighIndex + bar_index) / 2 , Major_HighLevel   , 
             text = 'Major ChoCh' , color = color.rgb(0,0,0,100), textcolor = MajorBuChoChLine_Color ,size = size.normal )
else
    Bullish_Major_ChoCh := false
    Bullish_Major_BoS   := false 


if  ta.crossunder(close, Major_LowLevel) and  LockBreak_M!= Major_LowIndex // Bearish BoS Detector
    if ExternalTrend == 'No Trend' or ExternalTrend == 'Down Trend'
        Bearish_Major_BoS := true
        BoS_MajorType.push('Bear Major BoS')
        BoS_MajorIndex.push(bar_index)
        LockBreak_M := Major_LowIndex
        ExternalTrend := 'Down Trend'
        if MajorBeBoSLine_Show == 'On'
            MajorLine_BoSBear     := line.new(Major_LowIndex, Major_LowLevel , bar_index , Major_LowLevel , style = MajorBeBoSLine_Style , color = MajorBeBoSLine_Color)
            MajorLabel_BoSBear    := label.new((Major_LowIndex + bar_index) / 2 , Major_LowLevel   , 
             text = 'Major BoS' , color = color.rgb(0,0,0,100), 
             textcolor = MajorBeBoSLine_Color , style = label.style_label_up ,size = size.normal)        
    else if ExternalTrend == 'Up Trend' // Bearish ChoCh Detector
        Bearish_Major_ChoCh := true
        ChoCh_MajorType.push('Bear Major ChoCh')
        ChoCh_MajorIndex.push(bar_index)
        LockBreak_M := Major_LowIndex
        ExternalTrend := 'Down Trend'
        if MajorBeChoChLine_Show == 'On'
            MajorLine_ChoChBear    := line.new(Major_LowIndex, Major_LowLevel , bar_index , Major_LowLevel , style = MajorBeChoChLine_Style , color = MajorBeChoChLine_Color)
            MajorLabel_ChoChBear   := label.new((Major_LowIndex + bar_index) / 2 , Major_LowLevel   , 
             text = 'Major ChoCh' , color = color.rgb(0,0,0,100), textcolor = MajorBeChoChLine_Color, style = label.style_label_up ,size = size.normal)
else 
    Bearish_Major_ChoCh := false 
    Bearish_Major_BoS   := false 


            //Minor

if  Minor_HighLevel < close  and  LockBreak_m != Minor_HighIndex // Bullish BoS Detector
    if (InternalTrend == 'No Trend' or InternalTrend == 'Up Trend') 
        Bullish_Minor_BoS   := true
        BoS_MinorType.push('Bull Minor BoS')
        BoS_MinorIndex.push(bar_index)
        LockBreak_m := Minor_HighIndex
        InternalTrend := 'Up Trend'
        if MinorBuBoSLine_Show  == 'On' 
            MinorLine_BoSBull     := line.new(Minor_HighIndex, Minor_HighLevel , bar_index , Minor_HighLevel , style = MinorBuBoSLine_Style , color = MinorBuBoSLine_Color)
            MinorLabel_BoSBull    := label.new((Minor_HighIndex + bar_index) / 2 , Minor_HighLevel   , text = 'Minor BoS' , color = color.rgb(0,0,0,100), textcolor = MinorBuBoSLine_Color ,size = size.small )        
    else if InternalTrend == 'Down Trend' // Bullish ChoCh Detector
        Bullish_Minor_ChoCh := true
        ChoCh_MinorType.push('Bull Minor ChoCh')
        ChoCh_MinorIndex.push(bar_index)
        LockBreak_m := Minor_HighIndex
        InternalTrend := 'Up Trend'
        if MinorBuChoChLine_Show  == 'On'         
            MinorLine_ChoChBull    := line.new(Minor_HighIndex, Minor_HighLevel , bar_index , Minor_HighLevel , style = MinorBuChoChLine_Style , color = MinorBuChoChLine_Color)
            MinorLabel_ChoChBull   := label.new((Minor_HighIndex + bar_index) / 2 , Minor_HighLevel   , text = 'Minor ChoCh' , color = color.rgb(0,0,0,100), textcolor = MinorBuChoChLine_Color ,size = size.small )
else 
    Bullish_Minor_ChoCh := false
    Bullish_Minor_BoS   := false


if  Minor_LowLevel > close and  LockBreak_m!= Minor_LowIndex // Bearish BoS Detector
    if InternalTrend == 'No Trend' or InternalTrend == 'Down Trend'
        Bearish_Minor_BoS   := true
        BoS_MinorType.push('Bear Minor BoS')
        BoS_MinorIndex.push(bar_index)
        LockBreak_m := Minor_LowIndex
        InternalTrend := 'Down Trend'
        if MinorBeBoSLine_Show  == 'On' 
            MinorLine_BoSBear     := line.new(Minor_LowIndex, Minor_LowLevel , bar_index , Minor_LowLevel , style = MinorBeBoSLine_Style , color = MinorBeBoSLine_Color)
            MinorLabel_BoSBear    := label.new((Minor_LowIndex + bar_index) / 2 , Minor_LowLevel   , text = 'Minor BoS' , color = color.rgb(0,0,0,100), 
             textcolor = MinorBeBoSLine_Color , style = label.style_label_up ,size = size.small) 
    else if InternalTrend == 'Up Trend' // Bearish ChoCh Detector
        Bearish_Minor_ChoCh := true
        ChoCh_MinorType.push('Bear Minor ChoCh')
        ChoCh_MinorIndex.push(bar_index)
        LockBreak_m := Minor_LowIndex
        InternalTrend := 'Down Trend'
        if MinorBeChoChLine_Show  == 'On' 
            MinorLine_ChoChBear    := line.new(Minor_LowIndex, Minor_LowLevel , bar_index , Minor_LowLevel , style = MinorBeChoChLine_Style , color = MinorBeChoChLine_Color)
            MinorLabel_ChoChBear   := label.new((Minor_LowIndex + bar_index) / 2 , Minor_LowLevel   , text = 'Minor ChoCh' , color = color.rgb(0,0,0,100), 
             textcolor = MinorBeChoChLine_Color, style = label.style_label_up ,size = size.small)
else
    Bearish_Minor_ChoCh := false
    Bearish_Minor_BoS   := false
