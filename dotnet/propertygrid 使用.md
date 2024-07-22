Introduction
PropertyGrid is a standard component windows form, which is in the first and second version of the .NET Framework. This component allows us to display properties of practically all types of objects, without writing any additional code. For all the demos, I created a Form and placed it on a button, with the button name btnAssign, and PropertyGrid with the name prpG.

The Simplest Way
In the simplest case, you write your classes by using properties, put them on the form PropertyGrid, and bind it's property SelectedObject reference to the object. You can then look in PropertiesGrid for any kind of properties of your object, writing something like this in buttonClick:

C#
PropertyGridSimpleDemoClass pgdc = new PropertyGridSimpleDemoClass();
prpG.SelectedObject = pgdc;
So, start a new windows project, place on the form button and PropertyGrid. In the property editor, assign btnAssig name to the button, and prpG name to PropertyGrid. After this, add the new class to the solution with the following code:

C#
Shrink ▲   
class PropertyGridSimpleDemoClass
{
    int m_DisplayInt;
    public int DisplayInt
    {
        get { return m_DisplayInt; }
        set { m_DisplayInt = value; }
    }

    string m_DisplayString;
    public string DisplayString
    {
        get { return m_DisplayString; }
        set { m_DisplayString = value; }
    }

    bool m_DisplayBool;
    public bool DisplayBool
    {
        get { return m_DisplayBool; }
        set { m_DisplayBool = value; }
    }

    Color m_DisplayColors;
    public Color DisplayColors
    {
        get { return m_DisplayColors; }
        set { m_DisplayColors = value; }
    }
}
After this, double click on the button and write the following code:

C#
PropertyGridSimpleDemoClass pgdc = new PropertyGridSimpleDemoClass();
prpG.SelectedObject = pgdc;
Run and compile the project, and press the button. You'll see something like this:

result
Using Attributes in PropertyGrid
In System.ComponentModel, there are some useful attributes. For example:

[Browsable(bool)] – to show property or not
[ReadOnly(bool)] – possibility to edit property
[Category(string)] – groups of property
[Description(string)] – property description. It is something like a hint.
[DisplayName(string)] – display property
If you use this class for assigning to SelectedObject:

C#
Shrink ▲   
class PropertyGridSimpleDemoClass2
{
    int m_DisplayInt = 50;                    // some initialization
    
    [Browsable(bool)]                         // this property should be visible
    [ReadOnly(true)]                          // but just read only
    [Description("sample hint1")]             // sample hint1
    [Category("Category1")]                   // Category that I want
    [DisplayName("Int for Displaying")]       // I want to say more, than just DisplayInt
    public int DisplayInt
    {
        get { return m_DisplayInt; }
        set { m_DisplayInt = value; }
    }

    string m_DisplayString;
    
    [Browsable(bool)]                            // this property should be visible
    [ReadOnly(false)]                            // this property is for editing
    [Description("Example Displaying hint 2")]   // sample hint2
    [Category("Category1")]                      // Category that I want
    [DisplayName("Name")]                        // and more than Display String
    public string DisplayString
    {
        get { return m_DisplayString; }
        set { m_DisplayString = value; }
    }

    bool m_DisplayBool;
    [Category("Category2")]                    // Category that I want
    [Description("To be or not to be")]        // yet one hint
    [DisplayName("To drink or not to drink")]  // that is a question
    public bool DisplayBool
    {
        get { return m_DisplayBool; }
        set { m_DisplayBool = value; }
    }

    Color m_DisplayColors;
    
false)>                           //this property should be hidden;
    public Color DisplayColors
    {
        get { return m_DisplayColors; }
        set { m_DisplayColors = value; }
    }
}
Then we get this screen:

result
More Complex or Better Looking True/False Values - It is Especially so for Bool Values
But sometimes, it is not enough to display just text values. Sometimes, we need a more impressive display. For example, if we want to change the variants in "To drink or not to drink" to other variants. Something like: “Yes” and “Yes, of course”. Than we need to use the attribute TypeConverter. First of all, we should create two additional classes. First we will name PropertyGridSimpleDemoClass3, another DrinkerClassConverter. Look at the class given below:

C#
Shrink ▲   
class PropertyGridSimpleDemoClass3
{
    bool m_DrinkOrNot;

    [DisplayName("Drink or not")]
    [Description("Drink or not")]
    [Category("Make right decision")]
    [TypeConverter(typeof(DrinkerClassConverter))]
    public bool DrinkOrNot
    {
        get { return m_DrinkOrNot; }
        set { m_DrinkOrNot = value; }
    }
}

class DrinkerClassConverter : BooleanConverter
{
    public override object ConvertTo(ITypeDescriptorContext context,
    CultureInfo culture,
    object value,
    Type destType)
    {
        return (bool)value ?
        "Yes" : "Yes, of course";
    }

    public override object ConvertFrom(ITypeDescriptorContext context,
    CultureInfo culture,
    object value)
    {
        return (string)value == "Yes";
    }
}
All that is needed is to use the Attribute TypeConverter, inherit BooleanConverter, and override ConvertTo and ConvertFrom. Then you see a result like the following:

result
Changing Standard Way of Displaying
Imagine that you want more than two items in a list and that the boolean type is not enough. What do you do then? First of all, you need to set the attribute Description with the needed name for every element in enu<code>m.

C#
enum DrinkDoses
{
    [Description("Half of litre")]
    litre,
    [Description("One litre")]
    oneLitre,
    [Description("Two litres")]
    twoLitre,
    [Description("Three litres")]
    threeLitres,
    [Description("Four litres")]
    fourLitres,
    [Description("Death dose, five litres")]
    fiveLitres
}
Secondly, you need to realize type EnumConverter.

C#
Shrink ▲   
class DrinkDosesConverter:EnumConverter
{
    private Type _enumType;
    /// <summary />Initializing instance</summary />
    /// <param name=""type"" />type Enum</param />
    /// this is only one function, that you must
    /// change. All another functions for enums
    /// you can use by Ctrl+C/Ctrl+V
    public DrinkDosesConverter(Type type)
        : base(type)
    {
        _enumType = type;
    }

    public override bool CanConvertTo(ITypeDescriptorContext context,
        Type destType)
    {
        return destType == typeof(string);
    }

    public override object ConvertTo(ITypeDescriptorContext context,
        CultureInfo culture,
        object value, Type destType)
    {
        FieldInfo fi = _enumType.GetField(Enum.GetName(_enumType, value));
        DescriptionAttribute dna =
            (DescriptionAttribute)Attribute.GetCustomAttribute(
            fi, typeof(DescriptionAttribute));

        if (dna != null)
            return dna.Description;
        else
            return value.ToString();
    }

    public override bool CanConvertFrom(ITypeDescriptorContext context,
        Type srcType)
    {
        return srcType == typeof(string);
    }

    public override object ConvertFrom(ITypeDescriptorContext context,
        CultureInfo culture,
        object value)
    {
        foreach (FieldInfo fi in _enumType.GetFields())
        {
            DescriptionAttribute dna =
            (DescriptionAttribute)Attribute.GetCustomAttribute(
            fi, typeof(DescriptionAttribute));

            if ((dna != null) && ((string)value == dna.Description))
                return Enum.Parse(_enumType, fi.Name);
        }
        return Enum.Parse(_enumType, (string)value);
    }
}
Finally, you need to set the attribute TypeConverter for displaying a property.

C#
Shrink ▲   
class DrinkerDoses
{
    DrinkDoses m_doses;
    [DisplayName("Doses")]
    [Description("Drinker doses")]
    [Category("Alcoholics drinking")]
    [TypeConverter(typeof(DrinkDosesConverter))]
    public DrinkDoses Doses
    {
        get
        {
            return m_doses;
        }
        set
        {
            m_doses = value;
        }
    }

    int m_dataInt;

    public int DataInt
    {
        get { return m_dataInt; }
        set { m_dataInt = value; }
    }
}
So, the result should look like this:

result
Displaying Pictures
Sometimes there arises a situation when you want to display pictures in PropertyGrid. Let's say we want to show in a game Stone, scissors, paper. Then first, what do we need to organize? We need to organize the enum, with attribute Description for every needed member:

C#
enum GameValues
{
    [Description("Stone")]
    Stone,
    [Description("Scissors")]
    Scissors,
    [Description("Paper")]
    Paper
}
After this, we realize EnumTypeConverter, that converts to a string by using the attribute Description.

C#
Shrink ▲   
class GameValuesConverter: EnumConverter
{
    private Type _enumType;

    public GameValuesConverter(Type type)
        : base(type)
    {
        _enumType = type;
    }

    public override bool CanConvertTo(ITypeDescriptorContext context,
        Type destType)
    {
        return destType == typeof(string);
    }

    public override object ConvertTo(ITypeDescriptorContext context,
        CultureInfo culture,
        object value, Type destType)
    {
        FieldInfo fi = _enumType.GetField(Enum.GetName(_enumType, value));
        DescriptionAttribute dna =
            (DescriptionAttribute) Attribute.GetCustomAttribute(
            fi, typeof(DescriptionAttribute));

        if (dna != null)
            return dna.Description;
        else
            return value.ToString();
    }

    public override bool CanConvertFrom(ITypeDescriptorContext context,
        Type srcType)
    {
        return srcType == typeof (string);
    }

    public override object ConvertFrom(ITypeDescriptorContext context,
        CultureInfo culture,
        object value)
    {
        foreach (FieldInfo fi in _enumType.GetFields())
        {
            DescriptionAttribute dna =
                (DescriptionAttribute) Attribute.GetCustomAttribute(
                fi, typeof(DescriptionAttribute));

            if ((dna != null) && ((string)value == dna.Description))
                return Enum.Parse(_enumType, fi.Name);
        }

        return Enum.Parse(_enumType, (string) value);
    }
}
This looks very similar to the previous step. I'll show you another sample. Now we need to organize the class, which contains an element which inherits UITypeEditor:

C#
class GameEditor: UITypeEditor
{
    public override bool GetPaintValueSupported(ITypeDescriptorContext context)
    {
        return true;
    }

    public override void PaintValue(PaintValueEventArgs e)
    {
        string whatImage = e.Value.ToString();
        whatImage += ".bmp";
        //getting picture
        Bitmap bmp = (Bitmap)Bitmap.FromFile(whatImage);
        Rectangle destRect = e.Bounds;
        bmp.MakeTransparent();

        //and drawing it
        e.Graphics.DrawImage(bmp, destRect);
    }
}
and bind it using attribute Editor to an editable field:

C#
class GameClassDisplayer
{
    GameValues m_GameValues;
    [DisplayName("Choose your variant")]
    [Description("You can choose between Stone, scissors, paper")]
    [Category("Choosing")]
    [Editor(typeof(GameEditor), typeof(UITypeEditor))]
    public GameValues DisplayGameValues
    {
        get
        {
            return m_GameValues;
        }
        set
        {
            m_GameValues = value;
        }
    }
}
After all this, you'll see:

result
