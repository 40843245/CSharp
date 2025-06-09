# CH6 -- properties in `Type` class
## objectives
You will learn

+ properties in `Type` class

## CH6.1 -- properties in `Type` class

In current version of [demo project](../demo%20project.md),

there are these properties

```
       //
        // 摘要:
        //     初始化 System.Type 類別的新執行個體。
        protected Type();

        //
        // 摘要:
        //     取得預設繫結器 (Binder) 的參考，它會實作內部規則來選取由 System.Type.InvokeMember(System.String,System.Reflection.BindingFlags,System.Reflection.Binder,System.Object,System.Object[],System.Reflection.ParameterModifier[],System.Globalization.CultureInfo,System.String[])
        //     呼叫的適當成員。
        //
        // 傳回:
        //     系統使用的預設繫結器之參考。
        public static Binder DefaultBinder { get; }
        //
        // 摘要:
        //     取得一組 System.Reflection.GenericParameterAttributes 旗標，敘述目前泛型類型參數的共變數與特殊條件約束。
        //
        // 傳回:
        //     System.Reflection.GenericParameterAttributes 值的位元組合，描述目前泛型型別參數的共變數和特殊條件約束。
        //
        // 例外狀況:
        //   T:System.InvalidOperationException:
        //     目前的 System.Type 物件不是泛型型別參數。 亦即，System.Type.IsGenericParameter 屬性會傳回 false。
        //
        //   T:System.NotSupportedException:
        //     基底類別不支援叫用的方法。
        public virtual GenericParameterAttributes GenericParameterAttributes { get; }
        //
        // 摘要:
        //     取得一個值，表示位於組件之外的程式碼是否能存取 System.Type。
        //
        // 傳回:
        //     如果目前 true 是公用類型或公用巢狀類型 (所有封入類型均為公用)，則為 System.Type，否則為 false。
        public bool IsVisible { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否未宣告為公用。
        //
        // 傳回:
        //     如果 true 尚未宣告為公用而且不是巢狀類型，則為 System.Type，否則為 false。
        public bool IsNotPublic { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否宣告為公用。
        //
        // 傳回:
        //     如果 true 已宣告為公用而且不是巢狀類型，則為 System.Type，否則為 false。
        public bool IsPublic { get; }
        //
        // 摘要:
        //     取得值，指出類別是否為巢狀 (Nest) 並且宣告為公用 (Public)。
        //
        // 傳回:
        //     如果類別是巢狀並且宣告為公用，則為 true，否則為 false。
        public bool IsNestedPublic { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為巢狀，並只在它自己的組件內為可見。
        //
        // 傳回:
        //     如果 true 是巢狀並只在它自己的組件內為可見，則為 System.Type，否則為 false。
        public bool IsNestedAssembly { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為巢狀，並只在它自己的系列內為可見。
        //
        // 傳回:
        //     如果 true 是巢狀並只在它自己的家族內為可見，則為 System.Type，否則為 false。
        public bool IsNestedFamily { get; }
        //
        // 摘要:
        //     取得與 System.Type 關聯的屬性。
        //
        // 傳回:
        //     代表 System.Reflection.TypeAttributes 屬性集的 System.Type 物件；若 System.Type 代表的是泛型型別參數，則這個值就是未指定的。
        public TypeAttributes Attributes { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為巢狀，並只對同時屬於它自己家族和它自己組件的類別為可見。
        //
        // 傳回:
        //     如果 System.Type 是巢狀並只對同時屬於它自己家族和它自己組件的類別為可見，則為 true，否則為 false。
        public bool IsNestedFamANDAssem { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為巢狀並只對屬於它自己家族或它自己組件的類別為可見。
        //
        // 傳回:
        //     如果 true 是巢狀並只對屬於它自己家族或它自己組件的類別為可見，則為 System.Type，否則為 false。
        public bool IsNestedFamORAssem { get; }
        //
        // 摘要:
        //     取得表示目前類型的欄位是否已由 Common Language Runtime 自動配置版面的值。
        //
        // 傳回:
        //     如果目前類型的 true 屬性包含 System.Type.Attributes 則為 System.Reflection.TypeAttributes.AutoLayout，否則為
        //     false。
        public bool IsAutoLayout { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為巢狀並且宣告為私用。
        //
        // 傳回:
        //     如果 true 是巢狀並且宣告為私用，則為 System.Type，否則為 false。
        public bool IsNestedPrivate { get; }
        //
        // 摘要:
        //     取得值，表示目前的 System.Type 物件代表的類型之定義是否位於另一個類型的定義內部。
        //
        // 傳回:
        //     如果 true 巢狀於另一個類型中，則為 System.Type，否則為 false。
        public bool IsNested { get; }
        //
        // 摘要:
        //     取得 System.Type 的命名空間。
        //
        // 傳回:
        //     System.Type 的命名空間；如果目前執行個體沒有命名空間或代表泛型參數，則為 null。
        public abstract string Namespace { get; }
        //
        // 摘要:
        //     取得目前 System.Type 所直接繼承的類型。
        //
        // 傳回:
        //     目前 System.Type 直接繼承自的 System.Type，如果目前 null 表示 Type 類別或介面，則為 System.Object。
        public abstract Type BaseType { get; }
        //
        // 摘要:
        //     取得類型的組件限定名稱，包含載入 System.Type 的組件名稱。
        //
        // 傳回:
        //     System.Type 的組件限定名稱，包含載入 System.Type 的組件名稱，如果目前的執行個體表示泛型類型參數，則為 null。
        public abstract string AssemblyQualifiedName { get; }
        //
        // 摘要:
        //     取得表示目前類型的欄位是否已依為其定義或發出至中繼資料之順序，循序配置版面的值。
        //
        // 傳回:
        //     如果目前類型的 true 屬性包含 System.Type.Attributes 則為 System.Reflection.TypeAttributes.SequentialLayout，否則為
        //     false。
        public bool IsLayoutSequential { get; }
        //
        // 摘要:
        //     取得類型的完整名稱 (包括其命名空間，但不包括其組件)。
        //
        // 傳回:
        //     類型的完整名稱 (包括其命名空間，但不包括其組件)；如果目前執行個體代表泛型類型參數、陣列類型、指標類型、根據類型參數的 null 類型，或不是泛型類型定義但包含未解析類型參數的泛型類型，則為
        //     byref。
        public abstract string FullName { get; }
        //
        // 摘要:
        //     取得目前 System.Type 的控制代碼。
        //
        // 傳回:
        //     目前 System.Type 的控制代碼。
        //
        // 例外狀況:
        //   T:System.NotSupportedException:
        //     .NET Compact Framework 目前不支援這個屬性。
        public virtual RuntimeTypeHandle TypeHandle { get; }
        //
        // 摘要:
        //     取得宣告此類型的 System.Reflection.Assembly。 若為泛型類型，則取得定義此泛型類型的 System.Reflection.Assembly。
        //
        // 傳回:
        //     System.Reflection.Assembly 執行個體，描述包含目前類型的組件。 若為泛型類型，則此執行個體描述的是含有泛型類型定義的組件，而不是建立與使用特定建構類型的組件。
        public abstract Assembly Assembly { get; }
        //
        // 摘要:
        //     在已定義的目前 System.Type 中取得模組 (DLL)。
        //
        // 傳回:
        //     在目前已定義之 System.Type 中的模組。
        public abstract Module Module { get; }
        //
        // 摘要:
        //     取得與 System.Type 相關聯的 GUID。
        //
        // 傳回:
        //     與 System.Type 相關聯的 GUID。
        public abstract Guid GUID { get; }
        //
        // 摘要:
        //     取得描述目前類型配置的 System.Runtime.InteropServices.StructLayoutAttribute。
        //
        // 傳回:
        //     取得描述目前類型概略配置特性的 System.Runtime.InteropServices.StructLayoutAttribute。
        //
        // 例外狀況:
        //   T:System.NotSupportedException:
        //     基底類別不支援叫用的方法。
        public virtual StructLayoutAttribute StructLayoutAttribute { get; }
        //
        // 摘要:
        //     取得用來取得這個成員的類別物件。
        //
        // 傳回:
        //     Type 物件，用來取得這個 System.Type 物件。
        public override Type ReflectedType { get; }
        //
        // 摘要:
        //     如果目前的 System.Reflection.MethodBase 表示泛型方法的類型參數，則取得表示宣告方法的 System.Type。
        //
        // 傳回:
        //     如果目前的 System.Type 表示泛型方法的類型參數，則為表示宣告方法的 System.Reflection.MethodBase否則為 null。
        public virtual MethodBase DeclaringMethod { get; }
        //
        // 摘要:
        //     取得宣告目前巢狀類型或泛型型別參數的類型。
        //
        // 傳回:
        //     若目前的類型是巢狀類型，即為表示封入類型的 System.Type 物件，若目前的類型是泛型類型的類型參數，則為泛型類型定義，而若目前的類型是泛型方法的類型參數，則為宣告泛型方法的類型，若以上皆否，便為
        //     null。
        public override Type DeclaringType { get; }
        //
        // 摘要:
        //     取得類型的初始設定式。
        //
        // 傳回:
        //     物件，包含 System.Type 的類別建構函式名稱。
        [ComVisible(true)]
        public ConstructorInfo TypeInitializer { get; }
        //
        // 摘要:
        //     取得表示目前類型的欄位是否已在明確指定之位移配置版面的值。
        //
        // 傳回:
        //     如果目前類型的 true 屬性包含 System.Type.Attributes 則為 System.Reflection.TypeAttributes.ExplicitLayout，否則為
        //     false。
        public bool IsExplicitLayout { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為實值類型。
        //
        // 傳回:
        //     如果 true 是實值類型，則為 System.Type，否則為 false。
        public bool IsValueType { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為介面；也就是說，不是類別或實值類型。
        //
        // 傳回:
        //     如果 true 是介面，則為 System.Type，否則為 false。
        public bool IsInterface { get; }
        //
        // 摘要:
        //     取得值，這個值表示目前類型在目前信任層級上是否為安全性安全關鍵，也就是說，它是否能執行重要作業並由安全性透明的程式碼存取。
        //
        // 傳回:
        //     如果目前類型在目前信任層級上為安全性安全關鍵，則為 true；如果為安全性關鍵或安全性透明，則為 false。
        public virtual bool IsSecuritySafeCritical { get; }
        //
        // 摘要:
        //     取得值，這個值表示目前類型在目前信任層級上是否為安全性關鍵或安全性安全關鍵，因而可以執行重要的作業。
        //
        // 傳回:
        //     如果目前類型在目前信任層級上為安全性關鍵或安全性安全關鍵，則為 true，如果是安全性透明，則為 false。
        public virtual bool IsSecurityCritical { get; }
        //
        // 摘要:
        //     取得此類型之泛型型別引數的陣列。
        //
        // 傳回:
        //     這個類型之泛型型別引數的陣列。
        public virtual Type [ ] GenericTypeArguments { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否以傳址方式封送處理。
        //
        // 傳回:
        //     如果 true 是以傳址方式進行封送處理，則為 System.Type，否則為 false。
        public bool IsMarshalByRef { get; }
        //
        // 摘要:
        //     取得值，指出在內容中是否可以裝載 System.Type。
        //
        // 傳回:
        //     如果在內容中可以裝載 System.Type，則為 true，否則為 false。
        public bool IsContextful { get; }
        //
        // 摘要:
        //     取得值，指出目前 System.Type 是否內含或參考其他類型；也就是說，目前 System.Type 是否為陣列、指標或以傳址方式傳遞。
        //
        // 傳回:
        //     如果 true 是陣列、指標或以傳址方式傳遞，則為 System.Type，否則為 false。
        public bool HasElementType { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為 COM 物件。
        //
        // 傳回:
        //     如果 true 是 COM 物件，則為 System.Type，否則為 false。
        public bool IsCOMObject { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為其中一個基本類型 (Primitive Type)。
        //
        // 傳回:
        //     如果 true 是其中一個基本類型，則為 System.Type，否則為 false。
        public bool IsPrimitive { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為指標。
        //
        // 傳回:
        //     如果 System.Type 是指標，則為 true，否則為 false。
        public bool IsPointer { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否以傳址方式傳遞。
        //
        // 傳回:
        //     如果 System.Type 是以傳址方式傳遞，則為 true，否則為 false。
        public bool IsByRef { get; }
        //
        // 摘要:
        //     取得值，該值指出目前的 System.Type 物件是否有尚未被特定類型取代的類型參數。
        //
        // 傳回:
        //     如果 true 物件本身為泛型類型參數或包含尚未提供特定類型的類型參數則為 System.Type否則為 false。
        public virtual bool ContainsGenericParameters { get; }
        //
        // 摘要:
        //     當 System.Type 物件表示泛型類型或泛型方法的型別參數時，在宣告參數的泛型類型或泛型方法之型別參數清單中，取得型別參數的位置。
        //
        // 傳回:
        //     型別參數在宣告參數的泛型類型或方法之型別參數清單中的位置。 位置編號從 0 開始。
        //
        // 例外狀況:
        //   T:System.InvalidOperationException:
        //     目前類型不代表型別參數。 亦即，System.Type.IsGenericParameter 會傳回 false。
        public virtual int GenericParameterPosition { get; }
        //
        // 摘要:
        //     取得值，指出目前的 System.Type 是否表示泛型類型或泛型方法定義中的型別參數。
        //
        // 傳回:
        //     如果 System.Type 物件表示泛型型別或泛型方法定義中的型別參數，則為 true，否則為 false。
        public virtual bool IsGenericParameter { get; }
        //
        // 摘要:
        //     取得值，指出這個物件是否表示建構的泛型類型。 您可以建立已建構之泛型類型的執行個體。
        //
        // 傳回:
        //     如果這個物件代表建構的泛型類型，則為 true，否則為 false。
        public virtual bool IsConstructedGenericType { get; }
        //
        // 摘要:
        //     取得值，指出目前的 System.Type 是否表示可用於建構其他泛型類型的泛型類型定義。
        //
        // 傳回:
        //     如果 true 物件表示泛型類型定義，則為 System.Type否則為 false。
        public virtual bool IsGenericTypeDefinition { get; }
        //
        // 摘要:
        //     取得值，指出目前類型是否為泛型類型。
        //
        // 傳回:
        //     true 如果目前的型別為泛型類型，否則， false。
        public virtual bool IsGenericType { get; }
        //
        // 摘要:
        //     取得值，以表示類型是否為陣列。
        //
        // 傳回:
        //     如果目前的類型是陣列則為 true，否則為 false。
        public bool IsArray { get; }
        //
        // 摘要:
        //     取得值，指出是否為 AutoClass 選取字串格式屬性 System.Type。
        //
        // 傳回:
        //     如果為 true 選取字串格式屬性 AutoClass，則為 System.Type，否則為 false。
        public bool IsAutoClass { get; }
        //
        // 摘要:
        //     取得值，指出是否為 UnicodeClass 選取字串格式屬性 System.Type。
        //
        // 傳回:
        //     如果為 true 選取字串格式屬性 UnicodeClass，則為 System.Type，否則為 false。
        public bool IsUnicodeClass { get; }
        //
        // 摘要:
        //     取得值，指出是否為 AnsiClass 選取字串格式屬性 System.Type。
        //
        // 傳回:
        //     如果為 true 選取字串格式屬性 AnsiClass，則為 System.Type，否則為 false。
        public bool IsAnsiClass { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否可序列化。
        //
        // 傳回:
        //     如果 System.Type 可序列化，則為 true，否則為 false。
        public virtual bool IsSerializable { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否套用了 System.Runtime.InteropServices.ComImportAttribute 屬性
        //     (Attribute)，亦即其是否從 COM 類型程式庫匯入。
        //
        // 傳回:
        //     如果 true 具有 System.Type，則為 System.Runtime.InteropServices.ComImportAttribute，否則為
        //     false。
        public bool IsImport { get; }
        //
        // 摘要:
        //     取得值，表示類型是否具有需要特殊處理的名稱。
        //
        // 傳回:
        //     如果類型具有需要特殊處理的名稱，則為 true，否則為 false。
        public bool IsSpecialName { get; }
        //
        // 摘要:
        //     取得值，指出目前的 System.Type 是否表示列舉類型。
        //
        // 傳回:
        //     如果目前 true 代表列舉，則為 System.Type，否則為 false。
        public virtual bool IsEnum { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否宣告為密封。
        //
        // 傳回:
        //     如果 true 宣告為密封，則為 System.Type，否則為 false。
        public bool IsSealed { get; }
        //
        // 摘要:
        //     取得值，指出 System.Type 是否為抽象並且必須被覆寫。
        //
        // 傳回:
        //     如果 System.Type 是抽象，則為 true，否則為 false。
        public bool IsAbstract { get; }
        //
        // 摘要:
        //     取得一個 System.Reflection.MemberTypes 值，代表這個成員是類型或巢狀類型。
        //
        // 傳回:
        //     一個 System.Reflection.MemberTypes 值，代表這個成員是類型或巢狀類型。
        public override MemberTypes MemberType { get; }
        //
        // 摘要:
        //     取得值，表示 System.Type 是類別或委派，也就是非實值類型或介面。
        //
        // 傳回:
        //     如果 System.Type 是類別，則為 true，否則為 false。
        public bool IsClass { get; }
        //
        // 摘要:
        //     取得值，這個值表示目前類型在目前信任層級上是否為透明，因此無法執行重要作業。
        //
        // 傳回:
        //     如果型別在目前信任層級上為安全性透明，則為 true，否則為 false。
        public virtual bool IsSecurityTransparent { get; }
        //
        // 摘要:
        //     指示類型，該類型是由表示這個類型的 Common Language Runtime 所提供的。
        //
        // 傳回:
        //     System.Type 的基礎系統類型。
        public abstract Type UnderlyingSystemType { get; }
```

## examples
### example 1
#### extension method
`TypeInfoExtensionMethods.GetPropertiesInfo` extension method is defined in `TypeInfoExtensionMethods` static class.

```
    public static class TypeInfoExtensionMethods
    {
        public static IndentationHandler indentationHandler = new IndentationHandler(0,' ','+');

        // ... omit other method for clarity

        public static string GetPropertiesInfo(
            this Type data,
            int indentationLevel = 0
        )
        {
            indentationHandler.IndentationLevel = indentationLevel;

            StringBuilder stringBuilder = new StringBuilder();

            stringBuilder.AppendFormat("The instance (is at IndentationLevel:{0}) with type `Type`:{0}\n" , indentationLevel , data.ToString());
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("Name:{0}\n") , data.Name);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("FullName:{0}\n") , data.FullName);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsPrimitive:{0}\n") , data.IsPrimitive);         
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsValueType:{0}\n") , data.IsValueType);         
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsArray:{0}\n") , data.IsArray);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsGenericType:{0}\n") , data.IsGenericType);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsEnum:{0}\n") , data.IsEnum);           
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsClass:{0}\n") , data.IsClass);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsAnsiClass:{0}\n") , data.IsAnsiClass);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsAutoLayout:{0}\n") , data.IsAutoLayout);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsUnicodeClass:{0}\n") , data.IsUnicodeClass);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsAutoClass:{0}\n") , data.IsAutoClass);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsInterface:{0}\n") , data.IsInterface);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsSealed:{0}\n") , data.IsSealed);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsPublic:{0}\n") , data.IsPublic);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNotPublic:{0}\n") , data.IsNotPublic);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsExplicitLayout:{0}\n") , data.IsExplicitLayout);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsLayoutSequential:{0}\n") , data.IsLayoutSequential);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNotPublic:{0}\n") , data.IsNotPublic);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNested:{0}\n") , data.IsNested);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedPublic:{0}\n") , data.IsNestedPublic);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedPrivate:{0}\n") , data.IsNestedPrivate);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedFamily:{0}\n") , data.IsNestedFamily);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedAssembly:{0}\n") , data.IsNestedAssembly);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedFamORAssem:{0}\n") , data.IsNestedFamORAssem);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsNestedFamANDAssem:{0}\n") , data.IsNestedFamANDAssem);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsSerializable:{0}\n") , data.IsSerializable);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsVisible:{0}\n") , data.IsVisible);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsCOMObject:{0}\n") , data.IsCOMObject);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsConstructedGenericType:{0}\n") , data.IsConstructedGenericType);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsContextful:{0}\n") , data.IsContextful);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsGenericType:{0}\n") , data.IsGenericType);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsMarshalByRef:{0}\n") , data.IsMarshalByRef);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsPointer:{0}\n") , data.IsPointer);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsByRef:{0}\n") , data.IsByRef);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsImport:{0}\n") , data.IsImport);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsSecurityCritical:{0}\n") , data.IsSecurityCritical);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsSecuritySafeCritical:{0}\n") , data.IsSecuritySafeCritical);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("IsSecurityTransparent:{0}\n") , data.IsSecurityTransparent);

            stringBuilder.AppendFormat("~~~~ end at IndentationLevel:{0}~~~~\n" , 0);
            return stringBuilder.ToString();
        }
    }
```

#### main code
Invoking following method

```
        /// <summary>
        ///  illustrate the properties of `Type` instance.
        /// </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Person personEli = new Person
            {
                FirstName = "Ayase" ,
                LastName = "Eli" ,
            };

            Employee employeeNico = new Employee
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
                Company = null
            };

            EmployeeRepository employeeRepository = new EmployeeRepository();
            GenericRepository<Person> personRepository = new GenericRepository<Person>();
            Func<int,GenericRepository<Person>> personRepositoryFunc = (i) => new GenericRepository<Person>();
            EmployeeService employeeService = new EmployeeService();
            GenericService genericService = new EmployeeService();
            Type [ ] datas = new Type [ ]
            {
                typeof(object),
                typeof(bool),
                typeof(List<int>),
                typeof(HashSet<int>),
                typeof(KeyValuePair<int,string>),
                typeof(Dictionary<int,string>),
                typeof(IList<int>),
                typeof(ICollection<int>),
                typeof(System.Console),
                typeof(Person),
                typeof(Employee),
                personNico.GetType(),
                employeeNico.GetType(),
                personEli.GetType(),
                personRepository.GetType(),
                personEli.GetType(),
                personRepositoryFunc.GetType(),
                employeeRepository.GetType(),
                employeeService.GetType(),
                genericService.GetType(),
            };

            foreach(var data in datas)
            {
                string infoText = data.GetPropertiesInfo();
                Console.WriteLine(infoText);
            }
        }
```

will output following

```
In TestMethod6 method call
The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Object
 + FullName:System.Object
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Boolean
 + FullName:System.Boolean
 + IsPrimitive:True
 + IsValueType:True
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:False
 + IsAnsiClass:True
 + IsAutoLayout:False
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:True
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:True
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:List`1
 + FullName:System.Collections.Generic.List`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:HashSet`1
 + FullName:System.Collections.Generic.HashSet`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:KeyValuePair`2
 + FullName:System.Collections.Generic.KeyValuePair`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:True
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:False
 + IsAnsiClass:True
 + IsAutoLayout:False
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:True
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:True
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Dictionary`2
 + FullName:System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:IList`1
 + FullName:System.Collections.Generic.IList`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:False
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:True
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:ICollection`1
 + FullName:System.Collections.Generic.ICollection`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:False
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:True
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Console
 + FullName:System.Console
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:True
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Employee
 + FullName:Example.DirtyBean.Employee
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Employee
 + FullName:Example.DirtyBean.Employee
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:GenericRepository`1
 + FullName:Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Func`2
 + FullName:System.Func`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]], Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:True
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:True
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:True
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:True
 + IsContextful:False
 + IsGenericType:True
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:False
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:True
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:EmployeeRepository
 + FullName:Example.Repositories.EmployeeRepository
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:True
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:EmployeeService
 + FullName:Example.Services.EmployeeService
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:EmployeeService
 + FullName:Example.Services.EmployeeService
 + IsPrimitive:False
 + IsValueType:False
 + IsArray:False
 + IsGenericType:False
 + IsEnum:False
 + IsClass:True
 + IsAnsiClass:True
 + IsAutoLayout:True
 + IsUnicodeClass:False
 + IsAutoClass:False
 + IsInterface:False
 + IsSealed:False
 + IsPublic:True
 + IsNotPublic:False
 + IsExplicitLayout:False
 + IsLayoutSequential:False
 + IsNotPublic:False
 + IsNested:False
 + IsNestedPublic:False
 + IsNestedPrivate:False
 + IsNestedFamily:False
 + IsNestedAssembly:False
 + IsNestedFamORAssem:False
 + IsNestedFamANDAssem:False
 + IsSerializable:False
 + IsVisible:True
 + IsCOMObject:False
 + IsConstructedGenericType:False
 + IsContextful:False
 + IsGenericType:False
 + IsMarshalByRef:False
 + IsPointer:False
 + IsByRef:False
 + IsImport:False
 + IsSecurityCritical:True
 + IsSecuritySafeCritical:False
 + IsSecurityTransparent:False
~~~~ end at IndentationLevel:0~~~~

```

## reference
### API docs
+ [`Type Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type?view=net-8.0)