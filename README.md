# LUA NA ZAKAZ

># Перечень основных функций

>### Main
- **GetDistance** - Получить дистацию до коодинат/юнита
    ```lua 
    distance = GetDistance(unit_a or(x,y,z), [unit_b or (x2,y2,z2)]) -- (NUMBER)
    -- Example:
    local dist = GetDistance("raid1", "boss1")
    if(dist < 10) then
        print("raid1 in range boss");
    end
    ```
- **GetDistance2D** - Получить дистацию до коодинат/юнита без учёта Z координаты
    ```lua 
    distance = GetDistance(unit_a or(x,y), [unit_b or (x2,y2)]) -- (NUMBER)
    -- Example:
    local dist = GetDistance2D("raid1", 100, 200)
    if(dist < 10) then
        print("raid1 in range coord [100, 200]");
    end
    ```
- **LineOfSight** - Проверка на LoS между юнитами/точками
    ```lua 
    in_los = LineOfSight(unit_a or(x,y,z), [unit_b or (x2,y2,z2)]) -- (BOOLEAN)
    -- Example:
    local in_los = LineOfSight("target");
    if (in_los) then
        Spell.Cast("Удар героя", "target");
    end
    ```
- **GetObjects** - Получение таблицы GUID'ов объектов
    ```lua 
    objects = GetObjects([table],[type_id, ...]) -- (TABLE)
    -- Example:
    local objects = GetObjects(); -- Получаем все объекты
    print("Обнаружено объектов:", #objects)
    GetObjects(objects); -- Перезаписываем таблицу objects объектами
    print("Обнаружено объектов:", #objects)
    GetObjects(objects, 4); -- Перезаписываем таблицу objects объектами, которые относятся к юнитам
    print("Обнаружено юнитов:", #objects)
    
    for _,obj in pairs(objects) do -- obj - это гуид, который используется как unitId.
        
        local dist = GetDistance(obj)
        if(dist <= 6 ) then
            print(UnitName(obj), "In MEELE")
        else
            print(UnitName(obj), "In RANGE")
        end
    end
    ```
- **GetObjectsCount** - Получение количества объектов в зоне видимости клиента
    ```lua 
    obj_count = GetObjectsCount() -- (NUMBER)
    -- Example:
    local obj_count = GetObjectsCount();
    print("Обнаружено объектов:", obj_count)
    ```
- **SetConsoleState** - Включить/Выключить консоль Innkeeper'а
    ```lua 
    SetConsoleState(boolean)
    ```
- **InitObjectManager** - Инициализация самообновляемой таблицы с объектами.<br>Объекты предоставлены в виде классов.
    ```lua 
    ObjectManager = InitObjectManager() -- (TABLE)
    -- Example:
    local om = InitObjectManager();
    for _, object in pairs(om.units) do
        print(object:Name())
    end
        ```

>### Directory
- **Init** - Инициализация класса Directory (создаёт папку, если она не существует).
    ```lua 
    directory = Directory.Init(path) -- (USERDATA)
    -- Example:
    local directory = Directory.Init("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    for _, v in pairs(directory:GetFiles()) do
        print(v:GetName())
    end
    ```
- **Get** - Инициализация класса Directory (только если папка существует).
    ```lua 
    directory = Directory.Get(path) -- (USERDATA)
    -- Example:
    local directory = Directory.Get("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    for _, v in pairs(directory:GetFiles()) do
        print(v:GetName())
    end
    ```
- **New** - Создание папки по указаному пути.
    ```lua 
    is_created = Directory.New(path, name) -- (BOOLEAN)
    -- Example:
    local is_created = Directory.New("C:\\Game\\WoW\\Interface\\Addons\\MyAddon", "SETTINGS")
    if(is_created) then
        print("Папка успешно создана");
    end
    ```
- **Delete** - Удаление папки по указаному пути.
    ```lua 
    is_deleted = Directory.Delete(path) -- (BOOLEAN)
    -- Example:
    local is_deleted = Directory.Delete("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    if(is_deleted) then
        print("Папка успешно удалена");
    end
    ```
- **Exists**: Проверяет, существует ли папка.
    ```lua 
    exists = Directory.Exists(path) -- (BOOLEAN)
    -- Example:
    local exists = Directory.Exists("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    if(exists) then
        print("Папка существует");
    end
    ```
- **GetWoWDirectory**: Возвращает путь к папке с игрой.
    ```lua 
    wow_path = Directory.GetWoWDirectory(path) -- (STRING)
    -- Example:
    local wow_path = Directory.GetWoWDirectory()
    print(wow_path) -- C:\\Game\\WoW
    ```

>### File

- **Init** - Инициализация класса File (создаёт файл, если он не существует).
    ```lua 
    file = File.Init(path) -- (USERDATA)
    -- Example:
    local file = File.Init("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    file:Load();
    ```
- **Get** - Инициализация класса File (только если файл существует).
    ```lua 
    file = File.Get(path) -- (USERDATA)
    -- Example:
    local file = File.Get("C:\\Game\\WoW\\Interface\\Addons\\MyAddon")
    file:Load();
    ```
- **New** - Создание файл по указаному пути.
    ```lua 
    is_created = File.New(path, name) -- (BOOLEAN)
    -- Example:
    local is_created = File.New("C:\\Game\\WoW\\Interface\\Addons\\MyAddon", "SETTINGS.lua")
    if(is_created) then
        print("Файл успешно создана");
    end
    ```
- **Delete** - Удаление файла по указаному пути.
    ```lua 
    is_deleted = File.Delete(path) -- (BOOLEAN)
    -- Example:
    local is_deleted = File.Delete("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\SETTINGS.lua")
    if(is_deleted) then
        print("Файл успешно удалён");
    end
    ```
- **Exists** - Проверяет, существует ли файл.
    ```lua 
    exists = File.Exists(path) -- (BOOLEAN)
    -- Example:
    local exists = File.Exists("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\SETTINGS.lua")
    if(exists) then
        print("Файл существует");
    end
    ```
- **Load** - Производит загрузку файла в lua игры
    ```lua 
    success, result = File.Load(path, [args, ...]) -- (BOOLEAN, value)
    -- Example:
    local success, result = File.Load("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\MyLib.lua")
    if(success) then
        print("Файл успешно выполнен");
    else
        print("Обнаружена ошибка:", result)
    end
    ```
- **LoadAsValue** - Производит загрузку файла в lua игры, в качестве значения.
    ```lua 
    success, result = File.LoadAsValue(path) -- (BOOLEAN, value)
    -- Example:
    local mySetting = {};
    local success, result = File.LoadAsValue("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\SETTINGS.lua")
    if(success) then
        mySetting = result;
        -- Дальше используем по своему усмотрению
    else
        print("Обнаружена ошибка:", result)
    end
    ```
- **Write** - Полная перезапись файла.
    ```lua 
    is_writen = File.Write(path, values[,...]) -- (BOOLEAN)
    -- Example:
    local is_writen = File.Write("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\SETTINGS.lua",values, ...)
    if(is_writen) then
        print("Файл успешно перезаписан");
    end
    ```
- **WriteLine** - Запись в конец файла.
    ```lua 
    is_writen = File.WriteLine(path, values[,...]) -- (BOOLEAN)
    -- Example:
    local is_writen = File.WriteLine("C:\\Game\\WoW\\Interface\\Addons\\MyAddon\\SETTINGS.lua", values, ...)
    if(is_writen) then
        print("Стрка успешно добавлена в конец файла");
    end
    ```
    
>### Memory

- **Read** - Прочитать память по указанному адресу.
    ```lua 
    value = Memory.Read("memory_types", pointer[, offset]) -- (MemoryTypes)
    -- Example:
    local value = Memory.Read("int", 0x12345678)
    print(value)
    ```
- **Write** - Записать в память по указанному адресу.
    ```lua 
    Memory.Write("memory_types", pointer, value) -- (VIOD)
    -- Example:
    Memory.Write("int", 0x12345678, 112)
    ```
- **Alloc** - Выделить память с указанным размером.
    ```lua 
    ptr = Memory.Alloc(size_t) -- (NUMBER)
    -- Example:
    local ptr = Memory.Alloc(4)
    print("Выделенный участок памяти находит по адресу:", ptr)
    ```
- **Free** - Освободить участок памяти
    ```lua 
    success = Memory.Free(pointer) -- (BOOLEAN)
    -- Example:
    local success = Memory.Free(0x12345678)
    if(success) then
        print("Пямять успешно освобождена")
    end
    ```
    
>### Spell

- **Init** - Инициализация класса Spell
    ```lua 
    spell = Spell.Init(spell_name or spell_id) -- (USERDATA)
    -- Example:
    local spell = Spell.Init(1234)
    if(spell:IsReady()) then
        print("Заклинание готово");
    end
    ```

- **GetMechanic** - Получить ID мехакини заклинания
    ```lua 
    success = Spell.GetMechanic(spellId or spellName) -- (NUMBER)
    -- Example:
    local mechanic = Spell.GetMechanic("Щит молний")
    print("Способность Щит молний имеет механику", mechanic)
    ```

- **GetEffect** - Получить ID эффектов заклинания
    ```lua 
    effect1, effect2, effect3 = Spell.GetEffect(spellId or spellName) -- (NUMBER), (NUMBER), (NUMBER)
    -- Example:
    local effect1, effect2, effect3 = Spell.GetEffect("Щит молний")
    print("Способность Щит молний имеет эффекты", effect1, effect2, effect3)
    ```

- **GetRange** - Получить дистранцию заклинания
    ```lua 
    min, max = Spell.GetRange(spellId or spellName, [unit]) -- (NUMBER), (NUMBER)
    -- Example:
    local min, max = Spell.GetRange("Свет небес", "raid1")
    local dist = GetDistacne("raid1")
    if(min < dist and max > dist) then
        Spell.Cast("Свет небес", "raid1")
    end
    ```

- **GetCooldown** - Время восстановления способности
    ```lua 
    cd_second = Spell.GetCooldown(spellId or spellName) -- (NUMBER)
    -- Example:
    local cd_second = Spell.GetCooldown(1234)
    if(cd_second > 0) then
        print("Заклинание не доступно ещё", cd_second , "секунд")
    end
    ```

- **GetName** - Получить Имя заклинания
    ```lua 
    name = Spell.GetName(spellId) -- (STRING)
    -- Example:
    local name = Spell.GetName(1234)
    print("Имя заклинания:", name)
    ```

- **IsReady** - Проверка на готовность спобоности к применению.
    ```lua 
    is_ready = Spell.IsReady(spellId or spellName, [isPet]) -- (BOOLEAN)
    -- Example:
    local is_ready = Spell.IsReady(1234, true)
    if(is_ready) then
        print("Заклинание питомца (1234) готово к применению")
    end
    ```

- **IsUsable** - Проверка можно ли применить способность.
    ```lua 
    is_usable = Spell.IsUsable(spellId or spellName, [isPet]) -- (BOOLEAN)
    -- Example:
    local is_usable = Spell.IsUsable(1234)
    if(is_usable) then
        print("Способность можно применить")
    end
    ```

- **GetInfoBySpellBookIndex** - Освободить участок памяти

    <details>
    <table>
        <thead>
            <tr>
                <th>Значение</th>
                <th>Type</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>entry</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>name</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>sub_text</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>cast_time</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>power_cost</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>mechanic</td>
                <td>NUMBER</td>
            </tr>
        </tbody>
    </table>
    </details>

    ```lua 
    entry, name, sub_text, cast_time, power_cost, mechanic = Spell.GetInfoBySpellBookIndex(index) -- (NUMBER), (STRING), (STRING), (NUMBER), (NUMBER), (NUMBER)
    -- Example:
    local entry, name, sub_text, cast_time, power_cost, mechanic = Spell.GetInfoBySpellBookIndex(1)
    print("Первое заклинание в книге: ", name)

    ```

- **Cast** - Применение заклинания
    ```lua 
    success = Spell.Cast(spellId or spellName, unitId) -- (BOOLEAN)
    -- Example:
    local success = Spell.Cast(1234, "target")
    if(success) then
        print("Способность успешно применена на target")
    end
    ```

- **CastAt** - Применение заклинания по области.
    ```lua 
    success = success = Spell.CastAt(spellId or spellName, unitId or x,y,z) -- (BOOLEAN)
    -- Example:
    local success = Spell.CastAt(1234, 123, 332, 112)
    if(success) then
        print("Способность успешно применена по координатам [123, 332, 112]")
    end
    ```

- **FlySpeed** - Скорость полёта заклинания.
    ```lua 
    fly_speed = Spell.FlySpeed(spellId or spellName) -- (NUMBER)
    -- Example:
    local fly_speed = Spell.FlySpeed(1234);
    local distance = GetDistance("target");
    if(fly_speed > 0) then
        print("Заклинание поразит цель через:", distance/fly_speed, "сек")
    end
    ```
- **StopCasting** - Отмена применения заклинания.
    ```lua 
    is_stoping = Spell.StopCasting() -- (BOOLEAN)
    -- Example:
    local is_stoping = Spell.StopCasting();
    if(is_stoping) then
        print("Заклинание успешно прерваною.")
    end
    ```
- **GetRadius** - Возвращает радиус действия АоЕ заклилания.
    ```lua 
    radius, max_radius = Spell.GetRadius(spell_id or spell_name) -- (NUMBER), (NUMBER)
    -- Example:
    local radius, max_radius = Spell.GetRadius(47820);
    if(radius) then
        print("Заклинание заденет всех игроков в радиусе:", radius)
    end
    ```

>### Object

- **Interact** - Взаимодействие с объектом
    ```lua 
    success = Object.Interact(objectId) -- (BOOLEAN)
    -- Example:
    local success = Object.Interact("target")
    if(success) then
        print("Успешное взаимодействие с целью")
    end
    ```

- **GetPosition** - Получить координаты объекта
    ```lua 
    x,y,z = Object.GetPosition(objectId) -- (NUMBER), (NUMBER), (NUMBER)
    -- Example:
    local x,y,z = Object.GetPosition("target")
    print("Цель находится по координатам:", x,y,z)
    ```

- **GetSize** - Получить размер объекта
    ```lua 
    size = Object.GetSize(objectId) -- (NUMBER)
    -- Example:
    local size = Object.GetSize("raid1")
    print("Размер модели:", size)
    ```

- **GetEntry** - Получить номер объекта
    ```lua 
    entry = Object.GetEntry(objectId) -- (NUMBER)
    -- Example:
    local entry = Object.GetEntry("boss1")
    if(entry == 12345) then
        print("В бою с боссом 12345")
    end
    ```

- **GetPointer** - Получить указать на объект в памяти.
    ```lua 
    pointer = Object.GetPointer(objectId) -- (NUMBER)
    -- Example:
    local pointer = Object.GetPointer("target")
    local speed = Memory.Read("float", pointer+ 0x814);
    print("Target Speed:", speed);
    ```


>### Unit

- **IsBehind** - Проверка, что цель стоит спиной к персонажу.
    ```lua 
    success = Unit.IsBehind(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsBehind("target")
    if(success) then
        print("Цель стоит спиной")
    end
    ```    
- **IsFacing** - Проверка, что цель стоит лицом к персонажу.
    ```lua 
    is_facing = Unit.IsFacing(UnitId) -- (BOOLEAN)
    -- Example:
    local is_facing = Unit.IsFacing("target")
    print("Цель находится по координатам:", x,y,z)
    ```
- **GetFacing** - Получить в какую угол, куда смотрит цель
    ```lua 
    facing = Unit.GetFacing(UnitId) -- (NUMBER)
    -- Example:
    local facing = Unit.GetFacing("raid1")
    print("Цель смотрит в:", size)
    ```    
- **IsInFront** - Проверка, что цель перед персонажем.
    ```lua 
    in_front = Unit.IsInFront(UnitId) -- (BOOLEAN)
    -- Example:
    local in_front = Unit.IsInFront("boss1")
    if(in_front) then
        print("Босс1 перед персожем, можно применять способности")
    end
    ```    
- **GetSideFacing** - Получения информации о том, с какой стороны указанный юнит
    ```lua 
    facing = Unit.GetSideFacing(UnitId) -- (NUMBER)
    -- Example:
    local facing = Unit.GetSideFacing("target")
    print("Target facing:", facing);
    ```
- **GetHitBoxSize** - Получить размер хитбокса
    ```lua 
    hit_box_size = Unit.GetHitBoxSize(UnitId) -- (BOOLEAN)
    -- Example:
    local hit_box_size = Unit.GetHitBoxSize("target")
    print("Цель можно атаковать с расстояния", 6 + hit_box_size)
    ```    
- **IsLootable** - Проверка, можно ли добычу с указанного юнита 
    ```lua 
    lootable = Unit.IsLootable(UnitId) -- (BOOLEAN)
    -- Example:
    local lootable = Unit.IsLootable("target")
    if(lootable)
        print("С цели можно подобрать добычу")
    end
    ```
- **IsLooting** - Проверка, происходит ли в данный момент сбор добычи с указанного юнита
    ```lua 
    looting = Unit.IsLooting(UnitId) -- (BOOLEAN)
    -- Example:
    local looting = Unit.IsLooting("target")
    if(looting) then
        print("Производится сбор добычи с target")
    end
    ```   
- **IsCasting** - Проверка, на применение заклинания юнитом
    ```lua 
    is_casting = Unit.IsCasting(UnitId) -- (BOOLEAN)
    -- Example:
    local is_casting = Unit.IsCasting("boss1")
    if(is_casting) then
        print("Босс что-то кастует!")
    end
    ```  
- **IsChanneling** - Проверка, на применение потокового заклинания юнитом
    ```lua 
    is_casting = Unit.IsChanneling(UnitId) -- (BOOLEAN)
    -- Example:
    local is_casting = Unit.IsChanneling("boss1")
    if(is_casting) then
        print("Босс кастует потоковое заклинание!")
    end
    ```
- **IsCastingOrChanneling** - Проверка, на применение обычного или потокового заклинания юнитом
    ```lua 
    is_casting = Unit.IsCastingOrChanneling(UnitId) -- (BOOLEAN)
    -- Example:
    local is_casting = Unit.IsCastingOrChanneling("boss1")
    if(is_casting) then
        print("Босс кастует!")
    end
    ```
- **GetCastingInfo** - Получить информацию о применяемом заклинении юнита
    <details>
    <summary>Перечень типов значений</summary>
    <table>
        <thead>
            <tr>
                <th>Значение</th>
                <th>Type</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Entry</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Name</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>SubText</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>Mechanic</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect1</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect2</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect3</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>End Casting Time</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Distance MIN</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Distance MAX</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>IsImmuneInterrupt</td>
                <td>BOOLEAN</td>
            </tr>
        </tbody>
    </table>
    </details>

    ```lua 
    entry, name, subtext, mechanic, effect1, effect2, effect3, end_casting_time, dist_min, dist_max, is_immune = Unit.GetCastingInfo(UnitId) -- (NUMBER), (STRING), (STRING), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (BOOLEAN)
    -- Example:
    local entry, name, subtext, mechanic, effect1, effect2, effect3, end_casting_time, dist_min, dist_max, is_immune = Unit.GetCastingInfo("target")
    if(entry == 1234 and not is_immune) then
        print("Рекомендуется сбить каст у target1");
    end
    ```
- **GetChannelingInfo** - Получить информацию о применяемом потоковом заклинении юнита
    <details>
    <summary>Перечень типов значений</summary>
    <table>
        <thead>
            <tr>
                <th>Значение</th>
                <th>Type</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Entry</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Name</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>SubText</td>
                <td>STRING</td>
            </tr>
            <tr>
                <td>Mechanic</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect1</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect2</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Effect3</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>End Casting Time</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Distance MIN</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>Distance MAX</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>IsImmuneInterrupt</td>
                <td>BOOLEAN</td>
            </tr>
        </tbody>
    </table>
    </details>

    ```lua 
    entry, name, subtext, mechanic, effect1, effect2, effect3, end_casting_time, dist_min, dist_max, is_immune = Unit.GetCastingInfo(UnitId) -- (NUMBER), (STRING), (STRING), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (NUMBER), (BOOLEAN)
    -- Example:
    local entry, name, subtext, mechanic, effect1, effect2, effect3, end_casting_time, dist_min, dist_max, is_immune = Unit.GetCastingInfo("target")
    if(entry == 1234 and not is_immune) then
        print("Рекомендуется сбить каст у target1");
    end
    ```
- **IsVendor** - Проверка, что цель является продавцом
    ```lua 
    success = Unit.IsVendor(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsVendor("target")
    if(success) then
        print("Цель является Vendor")
    end
    ```
- **IsVendorFood** - Проверка, что цель является продавцом еды
    ```lua 
    success = Unit.IsVendorFood(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsVendorFood("target")
    if(success) then
        print("Цель является VendorFood")
    end
    ```
- **IsVendorReagent** - Проверка, что цель является продавцом реагентов
    ```lua 
    success = Unit.IsVendorReagent(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsVendorReagent("target")
    if(success) then
        print("Цель является VendorReagent")
    end
    ```
- **IsRepairer** - Проверка, что цель является продавцом у которого можно починить вещи
    ```lua 
    success = Unit.IsRepairer(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsRepairer("target")
    if(success) then
        print("Цель является Repairer")
    end
    ```
- **IsClassTrainer** - Проверка, что цель является классовым тренером
    ```lua 
    success = Unit.IsFlightMaster(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsFlightMaster("target")
    if(success) then
        print("Цель является FlightMaster")
    end
    ```
- **IsProfessionTrainer** - Проверка, что цель является тренером профессий
    ```lua 
    success = Unit.IsProfessionTrainer(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsProfessionTrainer("target")
    if(success) then
        print("Цель является ProfessionTrainer")
    end
    ```
- **IsFlightMaster** - Проверка, что цель является учителем полётов
    ```lua 
    success = Unit.IsFlightMaster(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsFlightMaster("target")
    if(success) then
        print("Цель является FlightMaster")
    end
    ```
- **IsInnkeeper** - Проверка, что цель является хозяином таверны, к которому можно привязать камень возвращения
    ```lua 
    success = Unit.IsInnkeeper(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsInnkeeper("target")
    if(success) then
        print("Цель является Innkeeper")
    end
    ```
- **IsAuctioneer** - Проверка, что цель является аукционером
    ```lua 
    success = Unit.IsAuctioneer(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsAuctioneer("target")
    if(success) then
        print("Цель является Auctioneer")
    end
    ```
- **IsQuestGiver** - Проверка, у цели можно взять квест
    ```lua 
    success = Unit.IsQuestGiver(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsQuestGiver("target")
    if(success) then
        print("Цель является QuestGiver")
    end
    ```
- **IsGuildBanker** - Проверка, что цель является гильдейским банкиром
    ```lua 
    success = Unit.IsGuildBanker(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsGuildBanker("target")
    if(success) then
        print("Цель является GuildBanker")
    end
    ```
- **IsGossip** - Проверка, что с целью можно поговорить
    ```lua 
    success = Unit.IsGossip(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsGossip("target")
    if(success) then
        print("С целью можно поговорить")
    end
    ```
- **IsMailBox** - Проверка, что цель является почтой
    ```lua 
    success = Unit.IsMailBox(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsMailBox("target")
    if(success) then
        print("Цель является MailBox")
    end
    ```
- **IsInCombat** - Проверка, что цель в бою
    ```lua 
    success = Unit.IsInCombat(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsInCombat("target")
    if(success) then
        print("Цель в бою")
    end
- **IsMoving** - Проверка, что цель в движение
    ```lua 
    success = Unit.IsMoving(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsMoving("target")
    if(success) then
        print("Цель в движение")
    end
    ```
- **IsFalling** - Проверка, что цель падает/парит
    ```lua 
    success = Unit.IsFalling(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsFalling("target")
    if(success) then
        print("Цель в состояние падения")
    end
    ```
- **IsControlled** - Проверка, что цель под контроллем персонажа
    ```lua 
    success = Unit.IsControlled(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsControlled("target")
    if(success) then
        print("Цель контролем")
    end
    ```
- **IsDisarmed** - Проверка, что цель обезаружена
    ```lua 
    success = Unit.IsDisarmed(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsDisarmed("target")
    if(success) then
        print("Цель обезаружена")
    end
    ```
- **IsSilenced** - Проверка, что цель под безмолвием
    ```lua 
    success = Unit.IsSilenced(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsSilenced("target")
    if(success) then
        print("Цель под безмолвием")
    end
    ```
- **IsStunned** - Проверка, что цель под оглушением
    ```lua 
    success = Unit.IsStunned(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsStunned("target")
    if(success) then
        print("Цель в стане")
    end
    ```
- **IsServerControlled** - Проверка, что целью управляет сервер (контроль от мобов)
    ```lua 
    success = Unit.IsServerControlled(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsServerControlled("target")
    if(success) then
        print("Целью управляет сервер")
    end
    ```
- **IsConfused** - Проверка, что цель находится в замешательстве
    ```lua 
    success = Unit.IsConfused(unitId) -- (BOOLEAN)
    -- Example:
    local success = Unit.IsConfused("target")
    if(success) then
        print("Цель в замешательстве")
    end
    ```
- **HasMovementFlag** - Проверка, наличие Movement флага.
    ```lua 
    success = Unit.MovementHasFlag(unitId, flag) -- (BOOLEAN)
    -- Example:
    local success = Unit.MovementHasFlag("target", 0x8000)
    if(success) then
        print("Цель под флагом 0х8000")
    end
    ```
- **GetMovementFlag** - Получить флаг передвижения юнита.
    ```lua 
    flag = Unit.GetMovementFlag(unitId) -- (NUMBER)
    -- Example:
    local flag = Unit.GetMovementFlag("target")
    if(flag) then
        print("Флаг передвижения цели:", flag)
    end
    ```
- **GetTargetGUID** - Получить GUID таргета цели.
    ```lua 
    guid = Unit.GetTargetGUID(unitId) -- (HEX STRING)
    -- Example:
    local guid = Unit.GetTargetGUID("raid1")
    if(guid) then
        print("Raid1 атакует:", UnitName(guid))
    end
    ```
- **GetPetGUID** - Получить GUID питомца цели.
    ```lua 
    guid = Unit.GetPetGUID(unitId) -- (HEX STRING)
    -- Example:
    local guid = Unit.GetPetGUID("raid1")
    if(guid) then
        print("GUID питомца игрока raid1:", guid)
    end
    ```
- **GetCreatorGUID** - Получить GUID создателя юнита
    ```lua 
    guid = Unit.GetCreatorGUID(unitId) -- (HEX STRING)
    -- Example:
    local guid = Unit.GetCreatorGUID("target")
    if(guid) then
        print("Юнита создал:", UnitName(guid))
    end
    ```

>### Player

- **MoveToUnit** - Передвижение к цели
    ```lua 
    Player.MoveToUnit(unitId,[clickType]) -- (VOID)
    -- Example:
    Player.MoveToUnit("target", 0x06) -- Обыскать добычу
    ```
- **MoveToCoords** - Передвижение к координатам
    ```lua 
    Player.MoveToCoords(x,y,z) -- (VOID)
    -- Example:
    Player.MoveToCoords(Object.GetPosition("target"))
    ```
- **IsClickMoving** - Проверка, что персонаж сейчас бежит по коменде от Player.MoveTo
    ```lua 
    is_ctm = Player.IsClickMoving() -- (BOOLEAN)
    -- Example:
    local is_ctm = Player.IsClickMoving()
    if(is_ctm) then
        print("Передвижение по CTM")
    end
    ```
- **StopMoving** - Остановка передвижения
    ```lua 
    Player.StopMoving() -- (VOID)
    -- Example:
    if(GetDistacne("target") < 6 and Player.IsClickMoving()) then
        Player.StopMoving();
    end
    ```
- **SetFacing** - Повернуться в нужную радиану
    ```lua 
    Player.SetFacing(rad) -- (VOID)
    ```
- **LookAt** - Повернуться лицом к нужному объекту/юниту
    ```lua 
    Player.LookAt(objectId) -- (VOID)
    -- Example:
    Player.LookAt("raid1")
    ```
- **SetTarget** - Взять юнита в таргет.
    ```lua 
    Player.SetTarget(unitId) -- (VOID)
    -- Example:
    Player.SetTarget("raid1")

    ```

>### Item

- **GetItems** - Получить таблицу GUID'ов всех предметов
    ```lua
    items = Item.GetItems() -- (TABLE)
    --Example:
    local items = Item.GetItems();
    local items_by_set = 0;
    for i = 1, #items do
        local item_id = Object.GetEntry(items[i])
        local set_id = Item.GetSetId(item_id);
        if(set_id == 123) then
            items_by_set = items_by_set + 1;
        end
    end
    if(items_by_set >= 4) then
        print("Найдено 4 и более предмета из сета");
    end
    ```
- **GetCount** - Получить количество предметов (объектов)
    ```lua
    count = Item.GetCount() -- (NUMBER)
    --Example:
    local count = Item.GetCount();
    
    if(count < 1) then
        print("Предметы не обнаружены");
    else
        print("Обнаружено предметов:", count);
    end
    ```
- **GetSetId** - Получить SET_ID предмета
    ```lua
    set_id = Item.GetSetId(itemId) -- (NUMBER)
    --Example:
    local items = Item.GetItems();
    local items_by_set = 0;
    for i = 1, #items do
        local item_id = Object.GetEntry(items[i])
        local set_id = Item.GetSetId(item_id);
        if(set_id == 123) then
            items_by_set = items_by_set + 1;
        end
    end
    if(items_by_set >= 4) then
        print("Найдено 4 и более предмета из сета");
    end
    ```
- **GetIdByName** - Получить номер предмета по имени
    ```lua
    entry = Item.GetIdByName(item_name) -- (NUMBER)
    --Example:
    local entry = Item.GetIdByName("Громовая Ярость, благословенный клинок Искателя Ветра")
    print("Номер предмета:", entry) -- 19019
    ```
- **GetEnchantsCount** - Получить количество энчантов на надетом предмете
    ```lua
    ench_count = Item.GetEnchantsCount(slotId) -- (NUMBER)
    --Example:
    local ench_count = Item.GetEnchantsCount(16);
    if(ench_count < 1) then
        print("Чары не обнаружены на предмете")
    else
        for i = 1, ench_count, do
            local ench_id, ench_duration, ench_charges = Item.GetEnchantInfo(16, i);
            print(ench_id, ench_duration)
        end
    end
    ```
- **GetEnchantInfo** - Получить информацию об энчантах на надетом предмете
    ```lua
    ench_id, ench_duration, ench_charges = Item.GetEnchantInfo(slotId, index) -- (NUMBER), (NUMBER), (NUMBER)
    --Example:
    local ench_count = Item.GetEnchantsCount(16);
    if(ench_count < 1) then
        print("Чары не обнаружены на предмете")
    else
        for i = 1, ench_count, do
            local ench_id, ench_duration, ench_charges = Item.GetEnchantInfo(16, i);
            print(ench_id, ench_duration)
        end
    end
    ```
- **GetSpellInfo** - Получить информацию о заклинание у предмета.
    ```lua
    spell_id, spell_name, spell_rank, spell_subtext = Item.GetSpellInfo(itemId or itemName) -- (NUMBER), (STRING), (STRING), (STRING)
    --Example:
    local spell_id, spell_name, spell_rank, spell_subtext = Item.GetSpellInfo("Громовая Ярость, благословенный клинок Искателя Ветра")
    if(spell_id) then
        print("У предмета имеется следующее заклинание:", spell_name)
    end
    ```
- **CanUse** - Проверка, можно ли использовать предмет
    ```lua
    success = Item.CanUse(itemId or itemName) -- (BOOLEAN)
    --Example:
    local success = Item.CanUse("Зеркало Тералиона")
    if(success) then
        print("Предмет [Зеркало Тералиона] можно использовать")
    end
    ```
- **Use** - Использование предмета
    ```lua
    success = Item.Use(itemId or itemName, [unitId]) -- (BOOLEAN)
    --Example:
    local success = Item.Use("Зеркало Тералиона", "player")
    if(success) then
        print("Предмет [Зеркало Тералиона] было успешно использвано")
    end
    ```
- **GetNumEquippedItemsBySetId** - Проверка, количества надетых предметов по SEI_ID
    ```lua
    items_count = Item.GetNumEquippedItemsBySetId(set_id) -- (NUMBER)
    --Example:
    local items_count = Item.GetNumEquippedItemsBySetId(123)
    if(items_count >= 4) then
        print("На персонаже надето 4 или более предмета из сета 123")
    end
    ```
- **InitEquip** - Инициализация класса Equip
    ```lua
    equip = Item.InitEquip(slot_id) -- (USERDATA)
    --Example:
    local equip = Item.InitEquip(14)
    equip:Use("target");
    ```



>### DynamicObject

- **GetObjects** - Получить таблицу GUID'ов всех динамических объектов
    ```lua
    do_objects = DynamicObject.GetObjects() -- (TABLE)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        if(DynamicObject.IsMine(do_objects[i])) then
            print("Найден мой объект:", do_objects[i])
        end
    end
    ```
- **GetCount** - Получить количество объектов
    ```lua
    count = DynamicObject.GetCount() -- (NUMBER)
    --Example:
    local count = DynamicObject.GetCount();
    
    if(count < 1) then
        print("Динамические объекты не обнаружены");
    else
        print("Обнаружено объектов:", count);
    end
    ```
- **CasterGUID** - Получить GUID создателя объекта
    ```lua
    guid = DynamicObject.CasterGUID(objectId) -- (HEX STRING)
    --Example:
    local count = DynamicObject.GetCount();
    
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        local guid = DynamicObject.CasterGUID(do_objects[i]);
        print("Динамический объект создал:", UnitName(guid))
    end
    ```
- **IsMine** - Проверка, что создателем является player
    ```lua
    success = DynamicObject.IsMine(objectId) -- (BOOLEAN)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        if(DynamicObject.IsMine(do_objects[i])) then
            print("Найден мой объект:", do_objects[i])
        end
    end
    ```
- **GetRadius** - Получить радиус действия объекта
    ```lua
    radius = DynamicObject.GetRadius(objectId) -- (NUMBER)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        local radius = DynamicObject.GetRadius(do_objects[i])
        print("Радиус объекта:", radius)
    end
    ```
- **GetSpellId** - Получить spell_id объекта
    ```lua
    spell_id = DynamicObject.GetSpellId(objectId) -- (NUMBER)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        local spell_id = DynamicObject.GetSpellId(do_objects[i])
        print("Spell_ID объекта:", spell_id)
    end
    ```
- **GetCastTime** - Получить время, когда был создан объект
    ```lua
    cast_time = DynamicObject.IsMine(objectId) -- (NUMBER)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        local cast_time = DynamicObject.GetCastTime(do_objects[i])
        print("Объект создан:", cast_time)
    end
    ```
- **GetElapsed** - Получить время, коротое прошло с момента появления объекта
    ```lua
    elapsed = DynamicObject.GetElapsed(objectId) -- (NUMBER)
    --Example:
    local do_objects =  DynamicObject.GetObjects();
    for i = 1, #do_objects do
        local elapsed = DynamicObject.GetElapsed(do_objects[i])
        print("Прошло времени с момента создания:", elapsed)
    end
    ```

>### GameObject

- **GetObjects** - Получить таблицу GUID'ов всех игровых объектов
    ```lua
    go_objects = GameObject.GetObjects() -- (TABLE)
    --Example:
    local go_objects =  GameObject.GetObjects();
    for i = 1, #go_objects do
        if(GameObject.IsMine(go_objects[i])) then
            print("Найден мой объект:", go_objects[i])
        end
    end
    ```
- **GetCount** - Получить количество объектов
    ```lua
    count = GameObject.GetCount() -- (NUMBER)
    --Example:
    local count = GameObject.GetCount();
    
    if(count < 1) then
        print("Игровые объекты не обнаружены");
    else
        print("Обнаружено объектов:", count);
    end
    ```
- **GetType** - Получить тип игрового объекта
    ```lua
    obj_type = GameObject.GetType(objectId) -- (NUMBER)
    --Example:
    local obj_type = GameObject.GetType();
    
    if(count < 1) then
        print("Игровые объекты не обнаружены");
    else
        print("Обнаружено объектов:", count);
    end
    ```
- **CreatorGUID** - Получить GUID создателя объекта
    ```lua
    guid = GameObject.CasterGUID(objectId) -- (HEX STRING)
    --Example:
    local go_objects =  GameObject.GetObjects();
    for i = 1, #go_objects do
        local guid = GameObject.CasterGUID(go_objects[i])
        if(guid) then
            print("Объект создал", UnitName(guid))
        end
    end
    ```
- **IsMine** - Проверка, что создателем является player
    ```lua
    success = GameObject.IsMine(objectId) -- (BOOLEAN)
    --Example:
    local go_objects =  GameObject.GetObjects();
    for i = 1, #go_objects do
        if(GameObject.IsMine(go_objects[i])) then
            print("Найден мой объект:", go_objects[i])
        end
    end
    ```
- **GetFlag** - Получить flag игрового объекта
    ```lua
    obj_flag = GameObject.GetFlag(objectId) -- (NUMBER)
    --Example:
    local obj_flag = GameObject.GetFlag();
    print(obj_flag)
    ```
- **InLock** - Проверка блокировки игрового объекта
    ```lua
    success = GameObject.InLock(objectId) -- (BOOLEAN)
    --Example:
    local success = GameObject.InLock();
    
    if(success) then
        print("Игровой объект заблокирован")
    end
    ```
- **InUse** - Проверка использования игрового объекта
    ```lua
    success = GameObject.InUse(objectId) -- (BOOLEAN)
    --Example:
    local success = GameObject.InUse();
    if(success) then
        print("Игровой объект используется")
    end
    ```

>### Util
- **ResetAFKTimer** - Сброс AFK таймера
    ```lua
    Util.ResetAFKTimer() -- (VOID)
    ```
- **GetAsyncKeyState** - Проверка, зажата ли клавиша
    ```lua
    success, ... = Util.GetAsyncKeyState(KeyCode_1, ... , KeyCode_n) -- (BOOLEAN) (MULTI RETURN)
    -- Example:
    local success = Util.GetAsyncKeyState(0x70)
    if(success) then
        print("Клавиша F1 зажата")
    end
    ```
- **ProtectedCall** - Вызов оригинальной Blizzard функции из под защищённой системы (разблокированное LUA)
    ```lua
    values, ... = Util.ProtectedCall(function_name, [args,...]) -- (MULTI RETURN)
    -- Example:
    Util.ProtectedCall("CastSpellByName", "Свет небес", "target") -- Вызов функции CastSpellByName("Свет небес", "target")
    ```
- **TraceLine** - Проверка помех на пути виртуальной линии
    ```lua
    success, x, y, z = Util.TraceLine(unit_a or(x,y,z), [unit_b or (x2,y2,z2)], flag) -- (BOOLEAN), (NUMBER), (NUMBER), (NUMBER)
    -- Example:
    local tx,ty,tz = Object.GetPosition("target")
    local success, x, y, z = Util.TraceLine(tx,ty,tz, tx,ty,tz - 20,  0x100011)
    if(success) then
        Spell.CastAt(12345, x,y,z) -- применение способности по поверхности.
    end
    ```
- **FindPosition** - Поиск оптимальной позиции для применения заклинаний.
    ```lua
    count, x, y, z = Util.FindPosition(distance, radius, [array_members]) -- (NUMBER), (NUMBER), (NUMBER), (NUMBER)
    -- Example:
    local count, x, y, z = Util.FindPosition(40, 10)
    print(string.format("В координатах [X= %s, Y= %s, Z= %s] наибольшее скопление юнитов (%s шт.)", count, x, y, z))
    ```
- **RegisterHotKey** - Регистрация горячих клавиш.
    ```lua
    result = Util.RegisterHotKey(bind_name, keys, callback_func) -- (BOOLEAN)
    -- Example:
    local result = Util.RegisterHotKey("my_hotkey_bind", "ALT+F", function() print("PRESSED Alt & F") end)
    if(result) then
        print("ХотКей зарегистрирован")
    end
    ```
- **UnregisterHotKey** - Удаление ранее зарегистрированных горячих клавиш.
    ```lua
    result = Util.UnregisterHotKey(bind_name) -- (BOOLEAN)
    -- Example:
    local result = Util.UnregisterHotKey("my_hotkey_bind")
    if(result) then
        print("ХотКей удалён")
    end
    ```

</br> </br> </br>

># Перечень классов и их методов

> ### Класс File

- **GetName()** - Получить имя файла.
- **GetFullName()** - Получить имя файла с расширением.
- **GetPath()** - Получить путь к файлу.
- **GetExtension()** - Получить расширение файла.
- **Delete()** - Удалить файл.
- **Exists()** - Проверить, существует файл или нет.
- **Read()** - Прочитать файл, как текст.
- **Write(values[, ...])** - Перезаписать файл.
- **WriteLine(values[, ...])** - Добавить в конец файла.
- **Load(args[, ...])** - Загрузить файл в луа.
- **LoadAsValue()** - Загрузить файл как значение.

> ### Класс Directory

- **GetName()** - Получить имя папки.
- **GetPath()** - Получить путь к папке.
- **Delete()** - Удалить папку.
- **Exists()** - Проверить, существует папка или нет.
- **FileExists(file_name)** - Проверить, существует файл в папке или нет.
- **DirectoryExists(sub_dir_name)** - Проверить, существует подпапка в папке или нет.
- **GetFile(file_name)** - Получить класс File по имени файла.
- **GetFiles([extension])** - Получить таблицу файлов, которые иницилизированны как класс File
- **GetAllFiles([extension])** - Получить таблицу файлов, которые иницилизированны как класс File, сканирует с учётом подпапок.
- **GetDirectory(sub_dir_name)** - Получить класс Directory по имени подпапки.
- **GetDirectories()** - Получить таблицу подпапок, которые иницилизированны как класс Directory
- **CreateFile(file_name)** - Создание файла.
- **CreateDirectory(sub_dir_name)** - Создание папки.

> ### Класс ObjectManager

 *Аргументы которые указаны в квадратных скобках "[]" являются не обязательными, вместо них можно использовать nil*

</br>

- **__len()** - Получить количество объектов в массиве менеджера.
- **GetHealingMembers(filter_function [,table])** - Возвращает/Заполняет таблицу всех живых союзников по фильтру. *перед фильтрацией идёт проверка на лос, мертва цель, на цели нету духа(хприст)*
- **GetGroup([filter_function[, table [, can_scans_pets]]])** - Возвращает/Заполняет таблицу всех участников рейда/группы с петами.
- **GetEnemyUnits([table], [distance], [is_in_combat], [is_in_front])** - Возвращает/Заполняет таблицу с врагами, доп фильтры (дистанция, в бою, перед персонажем)
- **FindHealingPosition(radius, distance [, filter_function]])** - Возвращает лучшую позицию для применения AoE исцеления.
- **FindDamagePosition(radius, distance [, filter_function]])** - Возвращает лучшую  позицию для нанесение АоЕ урона.
- **Where(filter_function[, table])** - Возвращает/Заполняет таблицу с юнитами, по фильтации (данный метод так-же есть в подтаблицах).
- **First(filter_function)** - Возвращает первого юнита подходяшего по фильтру (данный метод так-же есть в подтаблицах).
- **RegisterObjectsEvent(func_name, event, function_callback)** - Регистрирует функцию колбека события OnCreate/OnDelete объектов.
- **UnregisterObjectsEvent(func_name, event)** - Удаляет функцию колбека события OnCreate/OnDelete объектов.

> ### Класс Object (*наследуемый*)

- **Exists()** - Проверка существование объекта.
- **GetName()** - Получить имя.
- **GetPosition()** - Получить координаты.
- **GetDistance([unit] or [x,y,z])** - Получить дистанцию.
- **GetGUID()** - Получить GUID.
- **GetEntry()** - Получить номер.
- **GetSize()** - Получить размер.
- **GetPointer()** - Получить указатель на объект в памяти.
- **IsOpenedLoot()** - Проверка, что открыт как лут.
- **IsInFront([unit])** - Проверка, что перед лицом игрока.
- **Interact()** - Взаимодействие с объектом.
- **GetFacing()** - Получить угол куда смотрит объект в радианах.
- **IsTransport()** - Проверить, что объект является транспортом.
- **CanBeTargetted()** - Проверить, что можно взять в таргет.
- **GetSpeed()** - Получить скорость передвижения.
- **GetTypeId()** - Получить тип объекта.
- **IsUnit()** - Проверка, что объект является Unit.
- **IsPlayer()** - Проверка, что объект является Player.
- **IsDynamicObject()** - Проверка, что объект является DynamicObject.
- **IsGameObject()** - Проверка, что объект является GameObject.
- **IsItem()** - Проверка, что объект является Item.
- **IsContainer()** - Проверка, что объект является Container.
- **IsCorpse()** - Проверка, что объект является Corpse.
- **InLoS([unit] or [x,y,z])** - Проверка, что объект в прямой видимости.
- **TraceLine([unit] or [x,y,z], flag)** - Проверка, на преграды до объекта, по указанному флагу.
- **HasFlag(flag)** - Проверка, на флаг объекта.

> ### Класс Unit :*Object*

- **CanAttack([unit])** - Проверяет, что юнита можно атаковать.
- **CanAssist([unit])** - Проверяет, что юниту можно помогать.
- **Reaction([unit])** - Возвращает реакцию юнита на игрока.
- **GetTargetGUID()** - Возвращает GUID таргета юнита.
- **GetTarget()** - Возвращает объект класса Unit таргет юнита.
- **Race()** - Возвращает расу юнита.
- **Class()** - Возвращает class_id юнита.
- **Gender()** - Возвращает пол юнита.
- **PowerType()** - Возвращает тип ресурса юнита.
- **Power()** - Возвращает количество ресурса у юнита.
- **Health()** - Возвращает уровень здоровья юнита.
- **Level()** - Возвращает уровень юнита.
- **IsCasting()** - Проверяет, юнит сейчас применяет заклинание или нет.
- **IsChanneled()** - Проверяет, юнит сейчас применяет потоковое заклинание или нет.
- **IsCastingOrChanneling()** - Проверяет, юнит сейчас применяет любое заклинание или нет.
- **GetCastingInfo()** - Возвращает информацию о заклинании, которое применяет юнит.
- **GetChannelingInfo()** - Возвращает информацию о потоковом заклинании, которое применяет юнит.
- **Creature()** - Возвращает тип существа.
- **IsDead()** - Проверяет, юнит мертвый или нет.
- **SummonedBy()** - Возвращает GUID юнита, который присумонил данного юнита.
- **MountId()** - Возвращает номер верхового животного у юнита.
- **GetPetGuid()** - Возвращает GUID его спутника/защитника.
- **GetPet()** - Возвращает объект класса Unit его спутника/защитника.
- **DynamicFlags()** - Возвращает динамический флаг юнита.
- **NpcFlags()** - Возвращает NPC флаг юнита.
- **IsTapped()** - Проверяет, был ли целевой юнит отмечен каким-либо игроком в группе или рейде.
- **IsTappedByMe()** - Проверяет, был ли целевой юнит отмечен текущим игроком.
- **InCombat()** - Проверяет, в бою юнит или нет.
- **IsLootable()** - Проверяет, доступность добычи с указанного юнита.
- **IsLooting()** - Проверяет, происходит ли в данный момент сбор добычи с указанного юнита.
- **IsImmuneInterrupt()** - Проверяет на иммунитет для прерывания заклинания.
- **IsInMounted()** - Проверяет, что юнит сейчас находится на маунте.
- **IsFalling()** - Проверяет, что юнит сейчас находится в падение/парении.
- **IsMoving()** - Проверяет, что юнит сейчас находится в движении.
- **GetMovementFlag()** - Возвращает флаг передвижения юнита.
- **HasMovementFlag(flag)** - Проверяет по флагу, на передвижение юнита.
- **IsVendor()** - Проверяет, юнит являет продавцом.
- **IsVendorFood()** - Проверяет, юнит являет продавцом еды.
- **IsVendorReagent()** - Проверяет, юнит являет продавцом реагентов.
- **IsRepairer()** - Проверяет, что у юнита можно починиться.
- **IsClassTrainer()** - Проверяет, что юнит является классовым тренером.
- **IsProfessionTrainer()** - Проверяет, что юнит является тренером профессий.
- **IsFlightmaster()** - Проверяет, что юнит является тренером полётов.
- **IsInnkeeper()** - Проверяет, что юнит является хозяином таверны.
- **IsAuctioneer()** - Проверяет, что юнит является аукционером.
- **IsGuildBanker()** - Проверяет, что юнит является гильдейским банкиром.
- **IsBanker()** - Проверяет, что юнит является банкиром.
- **IsStableMaster()** - Проверяет, что юнит является смотрителем стойла.
- **IsTotem()** - Проверяет, что юнит является тотемом.
- **IsGossip()** - Проверяет, находится ли юнит в состоянии разговора.
- **IsMailBox()** - Проверяет, что юнита можно использовать как почту.
- **IsSilenced()** - Проверяет, что юнит находится под воздействием безмолвия.
- **IsStunned()** - Проверяет, что юнит находится под воздействием оглушения.
- **IsConfused()** - Проверяет, что юнит находится под воздействием замещательства.
- **IsDisarmed()** - Проверяет, что юнит находится под воздействием обезоруживания.
- **IsControlled()** - Проверяет, что юнит находится под контролем игрока.
- **IsServerControlled()** - Проверяет, что юнит контролируется сервером.
- **HitBox()** - Получить размер хитбокса.
- **AurasCount()** - Получить количество аур на юните.
- **AuraByIndex(index)** - Возвращает информацию о ауре юнита по индексу.
- **Buff(spell_id or spell_name[, flag])** - Возвращает информацию о баффе юнита.
    ```lua
    id, name, rank, stacks, dispel_type_id, type_name, duration, time_to_end, caster, is_stealable = ObjectManager.unit:Buff(spell_id or spell_name [, filter]);
    --Example:
    local id, name, rank, stacks, dispel_type_id, dispel_name, duration, time_to_end, caster, is_stealable = ObjectManager.player:Buff(12345, "player");
    if(time_to_end and time_to_end < 10) then
        print("Пора обновить", name, "\nОсталось меньше "..time_to_end.." секунд.")
    end
    ```
- **Debuff(spell_id or spell_name[, flag])** - Возвращает информацию о дебаффе юнита.
    ```lua
    id, name, rank, stacks, dispel_type_id, type_name, duration, time_to_end, caster, is_stealable = ObjectManager.unit:Debuff(spell_id or spell_name [, filter]);
    --Example:
    local id, name, rank, stacks, dispel_type_id, dispel_name, duration, time_to_end, caster, is_stealable = ObjectManager.player:Debuff(12345);
    if(stacks and stacks > 10) then
       print("Набрано слишком много стаков", id);
    end
    ```
- **BuffCount(spell_id or spell_name[, flag])** - Возвращает информацию о количестве стаков баффа на юните.
- **DebuffCount(spell_id or spell_name[, flag])** - Возвращает информацию о количестве стаков дебафа на юните.
- **BuffTimer(spell_id or spell_name[, flag])** - Возвращает информацию об оставшейся длительности баффа на юните.
- **DebuffTimer(spell_id or spell_name[, flag])** - Возвращает информацию об оставшейся длительности дебаффа на юните.
- **HasBuff(spell_id or spell_name[, flag])** - Проверяет, наличие бафа на юните.
- **HasDebuff(spell_id or spell_name[, flag])** - Проверяет, наличие дебафа на юните.
- **CanDispelByType(types)** - Проверяет, что юнита можно продиспелить (можно объединять типы через симовл | или пробел).
- **CanPurgeByType(types)** - Проверяет, что юнита можно пропуржить (можно объединять типы через симовл | или пробел).
- **Select()** - Взять в таргет юнита.
- **IsBehind([unit])** - Проверяет, что юнит стоит спиной.
- **IsFacing[unit]** - Проверяет, что юнит стоит лицом.
- **InRaidOrParty()** - Проверяет, что юнит находится в рейде или пати.
- **GetUnitId()** - Получить игровой UnitId игрока. (защита от античита.)

> ### Класс Player :*Unit*
- **GetPlayerFlags()** - Получить PLAYER_FLAG игрока.
- **HasPlayerFlag(flag)** - Проверить наличине PLAYER_FLAG на игроке.

> ### Класс GameObject :*Object*

- **CreatedGUID()** - Получить GUID создателя.
- **CreatedUnit()** - Получить объект класса Unit создателя игрового объекта.
- **DisplayId()** - Получить ID модели внешнего вида.
- **Faction()** - Получить ID принадллежности игрового объекта.
- **Level()** - Получить уровень объекта.
- **IsLocked()** - Проверка, заблокирован объект или нет.
- **InUse()** - Проверка, объект в использование или нет.
- **IsMine()** - Проверка, объект создал персонаж или нет.
- **Flags()** - Получить флаги игрового объекта.
- **Type()** - Получить тип игрового объекта.

> ### Класс DynamicObject :*Object*

- **CreatedGUID()** - Получить GUID создателя.
- **CreatedUnit()** - Получить объект класса Unit создателя игрового объекта.
- **IsMine()** - Проверка, объект создал персонаж или нет.
- **GetEntry()** - Получить номер динамического объекта.
- **Radius()** - Получить радиус динамического объекта.
- **CastTime()** - Получить время, когда создался объект.
- **Elapsed()** - Получить время, которое прошло с момента создания объекта.

> ### Класс Item :*Object*

- **CreatorGUID()** - Получить GUID создателя.
- **OwnedGUID()** - Получить GUID владельца.
- **Stacks()** - Получить количество стаков.
- **CanEnchantmentId(enchant_id)** - Проверить, есть ли на предмете энчант.
- **EnchantInfo(enchant_idx)** - Получить информацию об энчанте.
- **EnchantsCount()** - Получить количество энчантов.
- **IsSoulbound()** - Проверка, персональный предмет или нет
- **CanUse()** - Проверить, можно использовать или нет.
- **GetSpellId()** - Получить spell_id у предмета.
- **GetItemFlag()** - Получить флаги предмета.
- **Use(unit)** - Использовать предмет.
- **Cooldown()** - Получить текущее время восстановление предмета.
- **GetSetId()** - Получить set_id предмета.
- **SlotId()** - Получить slot_id предмета
- **GetDisplayId()** - Получить display_id предмета.
- **ItemLvl()** - Получить информацию об уровне предмета.
- **SocketBonus()** - Получить информацию о секет бонусе.
- **Stackable()** - Проверить, можнет предмет стакаться или нет.
- **Quality()** - Получить качество предмета.
- **Bonding()** - Получить номер привязки предмета.

> ### Класс Equipped

- **CreatorGUID()** - Получить GUID создателя.
- **Stacks()** - Получить количество стаков.
- **CanEnchantmentId(enchant_id)** - Проверить, есть ли на предмете энчант.
- **EnchantInfo(enchant_idx)** - Получить информацию об энчанте.
- **EnchantsCount()** - Получить количество энчантов.
- **GetSpellId()** - Получить spell_id у предмета.
- **GetItemFlag()** - Получить флаги предмета.
- **Cooldown()** - Получить текущее время восстановление предмета.
- **CanUse()** - Проверить, можно использовать или нет.
- **Use(unit)** - Использовать предмет.
- **GetSetId()** - Получить set_id предмета.
- **GetDisplayId()** - Получить display_id предмета.
- **ItemLvl()** - Получить информацию об уровне предмета.
- **SocketBonus()** - Получить информацию о секет бонусе.
- **Quality()** - Получить качество предмета.
- **GetItemId()** - Получить номер предмета.
- **GetItemName()** - Получить имя предмета.

> ### Класс Spell

- **Mechanic()** - Получить механику.
- **Effect()** - Получить список эффектов.
- **Range([unit])** - Получить дальность применения.
- **Cooldown()** - Получить текущее время восстановления.
- **IsReady([is_pet])** - Проверка готовности.
- **IsUsable([is_pet])** - Проверка возможности использования.
- **InRange([unit])** - Проверка дистанции.
- **CastTime()** - Время применения заклинания.
- **PowerCost()** - Количество ресурса для применения.
- **IsCurrentSpell()** - Проверка, что сейчас применяется текущее заклинание.
- **GetEntry()** - Получить spell_id.
- **GetName()** - Получить имя.
- **DispelType()** - Получить информацию чем диспелить данное заклинание.
- **Description()** - Получить описание способности.
- **FlySpeed()** - Скорость полёта заклинания.
- **FlyTime(unit)** - Время полёта заклинания до цели.
- **SchoolMask()** - Маска школы заклинания.
- **PowerType()** - Тип требуемого ресурса.
- **RuneCostId()** - Получит rune_id для пременения заклинания.
- **DefenseType()** - Тип защиты.
- **IsHelpful()** - Положительное заклинание.
- **IsHarmful()** - Отрицательное заклинание.
- **Cast(unit [,last_cast_time])** - Применить заклинание на цель.
- **CastAt(unit or (x,y,z) [,last_cast_time])** - Применить заклинание по области.
- **GetLastCasting(unit)** - Возвращает время с момента последнего применения способности в указанного юнита (если применение было через функцию Spell:Cast(unit)).
- **IsGround()** - Проверка спобосности на применение по местности.
- **IsChanneled()** - Проверка, является способность потоковой.
- **CanCastOnMove()** Проверка, способность можно применить на ходу.
- **GetImplicitTargetA()** - Возрвщает 3 целевых значаний таргета A.
- **GetImplicitTargetB()** - Возрвщает 3 целевых значаний таргета B.
- **GetRadius()** - Возвращает радиус действия АоЕ заклинания.

> # Перечень типов, структур и флагов.

<details>
    <summary>Dispel Type_ID</summary>
    <table>
        <thead>
            <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Notes</th>
            </tr>
        </thead>
        <tbody>
            <tr>
            <td>0</td>
            <td>NONE</td>
            <td>CANT BE DISPELLED</td>
            </tr>
            <tr>
            <td>1</td>
            <td>MAGIC</td>
            <td>DISPELLED BY EFFECTS THAT DISPEL MAGIC</td>
            </tr>
            <tr>
            <td>2</td>
            <td>CURSE</td>
            <td>DISPELLED BY EFFECTS THAT DISPEL CURSES</td>
            </tr>
            <tr>
            <td>3</td>
            <td>DISEASE</td>
            <td>DISPELLED BY EFFECTS THAT DISPEL DISEASES</td>
            </tr>
            <tr>
            <td>4</td>
            <td>POISON</td>
            <td>DISPELLED BY EFFECTS THAT DISPEL POISONS</td>
            </tr>
            <tr>
            <td>5</td>
            <td>STEALTH</td>
            <td>DISPEL STEALTH EFFECTS?</td>
            </tr>
            <tr>
            <td>6</td>
            <td>INVISIBILITY</td>
            <td>DISPELS INVISIBILITY?</td>
            </tr>
            <tr>
            <td>7</td>
            <td>MAGIC, DISEASE, CURSE, POISON</td>
            <td>DISPELLED BY ANY OF THE 4 DISPEL METHODS</td>
            </tr>
            <tr>
            <td>8</td>
            <td>SPELL, NPC ONLY</td>
            <td>???</td>
            </tr>
            <tr>
            <td>9</td>
            <td>ENRAGE</td>
            <td>DISPELLED BY EFFECTS THAT DISPEL ENRAGE</td>
            </tr>
            <tr>
            <td>10</td>
            <td>ZULGURUB TRINKETS</td>
            <td>???</td>
            </tr>
            <tr>
            <td>11</td>
            <td>OLD, UNUSED</td>
            <td>???</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Class_ID</summary>
    <table>
        <thead>
            <tr>
                <th>Наименование</th>
                <th>Class ID</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>NONE</td>
                <td>0</td>
            </tr>
            <tr>
                <td>WARRIOR</td>
                <td>1</td>
            </tr>
            <tr>
                <td>PALADIN</td>
                <td>2</td>
            </tr>
            <tr>
                <td>HUNTER</td>
                <td>3</td>
            </tr>
            <tr>
                <td>ROGUE</td>
                <td>4</td>
            </tr>
            <tr>
                <td>PRIEST</td>
                <td>5</td>
            </tr>
            <tr>
                <td>DEATHKNIGHT</td>
                <td>6</td>
            </tr>
            <tr>
                <td>SHAMAN</td>
                <td>7</td>
            </tr>
            <tr>
                <td>MAGE</td>
                <td>8</td>
            </tr>
            <tr>
                <td>WARLOCK</td>
                <td>9</td>
            </tr>
            <tr>
                <td>MONK</td>
                <td>10</td>
            </tr>
            <tr>
                <td>DRUID</td>
                <td>11</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>WoWCreatureType</summary>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Value</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Unknown</td>
                <td>0</td>
            </tr>
            <tr>
                <td>Beast</td>
                <td>1</td>
            </tr>
            <tr>
                <td>Dragon</td>
                <td>2</td>
            </tr>
            <tr>
                <td>Demon</td>
                <td>3</td>
            </tr>
            <tr>
                <td>Elemental</td>
                <td>4</td>
            </tr>
            <tr>
                <td>Giant</td>
                <td>5</td>
            </tr>
            <tr>
                <td>Undead</td>
                <td>6</td>
            </tr>
            <tr>
                <td>Humanoid</td>
                <td>7</td>
            </tr>
            <tr>
                <td>Critter</td>
                <td>8</td>
            </tr>
            <tr>
                <td>Mechanical</td>
                <td>9</td>
            </tr>
            <tr>
                <td>NotSpecified</td>
                <td>10</td>
            </tr>
            <tr>
                <td>Totem</td>
                <td>11</td>
            </tr>
            <tr>
                <td>NonCombatPet</td>
                <td>12</td>
            </tr>
            <tr>
                <td>GasCloud</td>
                <td>13</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Перечень Reaction</summary>
    <table>
        <thead>
            <tr>
                <th>Наименование Reaction</th>
                <th>ReactionId</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>HATED</td>
                <td>0</td>
            </tr>
            <tr>
                <td>HOSTILE</td>
                <td>1</td>
            </tr>
            <tr>
                <td>UNFRIENDLY</td>
                <td>2</td>
            </tr>
            <tr>
                <td>NEUTRAL</td>
                <td>3</td>
            </tr>
            <tr>
                <td>FRIENDLY</td>
                <td>4</td>
            </tr>
            <tr>
                <td>HONORED</td>
                <td>5</td>
            </tr>
            <tr>
                <td>REVERED</td>
                <td>6</td>
            </tr>
            <tr>
                <td>EXALTED</td>
                <td>7</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>MovementFlags</summary>
    <table>
        <thead>
            <tr>
            <th>Имя</th>
            <th>ID</th>
            </tr>
        </thead>
        <tbody>
            <tr>
            <td>Forward</td>
            <td>0x1</td>
            </tr>
            <tr>
            <td>Backward</td>
            <td>0x2</td>
            </tr>
            <tr>
            <td>StrafeLeft</td>
            <td>0x4</td>
            </tr>
            <tr>
            <td>StrafeRight</td>
            <td>0x8</td>
            </tr>
            <tr>
            <td>Left</td>
            <td>0x10</td>
            </tr>
            <tr>
            <td>Right</td>
            <td>0x20</td>
            </tr>
            <tr>
            <td>PitchUp</td>
            <td>0x40</td>
            </tr>
            <tr>
            <td>PitchDown</td>
            <td>0x80</td>
            </tr>
            <tr>
            <td>Walk</td>
            <td>0x100</td>
            </tr>
            <tr>
            <td>TimeValid</td>
            <td>0x200</td>
            </tr>
            <tr>
            <td>Immobilized</td>
            <td>0x400</td>
            </tr>
            <tr>
            <td>DontCollide</td>
            <td>0x800</td>
            </tr>
            <tr>
            <td>Redirected</td>
            <td>0x1000</td>
            </tr>
            <tr>
            <td>Rooted</td>
            <td>0x2000</td>
            </tr>
            <tr>
            <td>Falling</td>
            <td>0x4000</td>
            </tr>
            <tr>
            <td>FallenFar</td>
            <td>0x8000</td>
            </tr>
            <tr>
            <td>PendingStop</td>
            <td>0x10000</td>
            </tr>
            <tr>
            <td>Pendingunstrafe</td>
            <td>0x20000</td>
            </tr>
            <tr>
            <td>Pendingfall</td>
            <td>0x40000</td>
            </tr>
            <tr>
            <td>Pendingforward</td>
            <td>0x80000</td>
            </tr>
            <tr>
            <td>PendingBackward</td>
            <td>0x100000</td>
            </tr>
            <tr>
            <td>PendingStrafeLeft</td>
            <td>0x200000</td>
            </tr>
            <tr>
            <td>PendingStrafeRght</td>
            <td>0x400000</td>
            </tr>
            <tr>
            <td>Moved</td>
            <td>0x800000</td>
            </tr>
            <tr>
            <td>Sliding</td>
            <td>0x1000000</td>
            </tr>
            <tr>
            <td>Swimming</td>
            <td>0x2000000</td>
            </tr>
            <tr>
            <td>SplineMover</td>
            <td>0x4000000</td>
            </tr>
            <tr>
            <td>SpeedDirty</td>
            <td>0x8000000</td>
            </tr>
            <tr>
            <td>Halted</td>
            <td>0x10000000</td>
            </tr>
            <tr>
            <td>Nudge</td>
            <td>0x20000000</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Movement ClickType</summary>
    <table>
        <thead>
            <tr>
                <th>Наименование действия</th>
                <th>TypeId</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>LEFT_CLICK</td>
                <td>0x01</td>
            </tr>
            <tr>
                <td>FACE</td>
                <td>0x02</td>
            </tr>
            <tr>
                <td>STOP_TROWN_EXCEPTION</td>
                <td>0x03</td>
            </tr>
            <tr>
                <td>MOVE</td>
                <td>0x04</td>
            </tr>
            <tr>
                <td>NPC_INTERACT</td>
                <td>0x05</td>
            </tr>
            <tr>
                <td>LOOT</td>
                <td>0x06</td>
            </tr>
            <tr>
                <td>OBJECT_INTERACT</td>
                <td>0x07</td>
            </tr>
            <tr>
                <td>FACE_OTHER</td>
                <td>0x08</td>
            </tr>
            <tr>
                <td>SKIN</td>
                <td>0x09</td>
            </tr>
            <tr>
                <td>ATTACK_POSITION</td>
                <td>0x0A</td>
            </tr>
            <tr>
                <td>ATTACK_GUID</td>
                <td>0x0B</td>
            </tr>
            <tr>
                <td>CONTACT_FACE</td>
                <td>0x0C</td>
            </tr>
            <tr>
                <td>NONE</td>
                <td>0x0D</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Memory Types</summary>
    <table>
        <thead>
            <tr>
                <th>Значение</th>
                <th>Type</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>byte</td>
                <td>HEX STRING</td>
            </tr>
            <tr>
                <td>int8</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>int16</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>int</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>float</td>
                <td>NUMBER</td>
            </tr>
            <tr>
                <td>int64</td>
                <td>HEX STRING</td>
            </tr>
            <tr>
                <td>string</td>
                <td>STRING</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Spell Mechanics</summary>
    <table>
        <thead>
            <tr>
                <th>Mechanics Name</th>
                <th>Mechanics Id</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>NONE</td>
                <td>0x00</td>
            </tr>
            <tr>
                <td>CHARMED</td>
                <td>0x01</td>
            </tr>
            <tr>
                <td>DISORIENTED</td>
                <td>0x02</td>
            </tr>
            <tr>
                <td>DISARMED</td>
                <td>0x03</td>
            </tr>
            <tr>
                <td>DISTRACTED</td>
                <td>0x04</td>
            </tr>
            <tr>
                <td>FLEEING</td>
                <td>0x05</td>
            </tr>
            <tr>
                <td>GRIP</td>
                <td>0x06</td>
            </tr>
            <tr>
                <td>ROOTED</td>
                <td>0x07</td>
            </tr>
            <tr>
                <td>SILENCED</td>
                <td>0x09</td>
            </tr>
            <tr>
                <td>ASLEEP</td>
                <td>0x0A</td>
            </tr>
            <tr>
                <td>ENSNARED</td>
                <td>0x0B</td>
            </tr>
            <tr>
                <td>STUNED</td>
                <td>0x0C</td>
            </tr>
            <tr>
                <td>FROZEN</td>
                <td>0x0D</td>
            </tr>
            <tr>
                <td>INCAPACITATED</td>
                <td>0x0E</td>
            </tr>
            <tr>
                <td>BLEED</td>
                <td>0x0F</td>
            </tr>
            <tr>
                <td>HEALING</td>
                <td>0x10</td>
            </tr>
            <tr>
                <td>POLYMORHPED</td>
                <td>0x11</td>
            </tr>
            <tr>
                <td>BANISHED</td>
                <td>0x12</td>
            </tr>
            <tr>
                <td>SHIELDED</td>
                <td>0x13</td>
            </tr>
            <tr>
                <td>SHACKLED</td>
                <td>0x14</td>
            </tr>
            <tr>
                <td>MOUNTED</td>
                <td>0x15</td>
            </tr>
            <tr>
                <td>SEDUCED</td>
                <td>0x16</td>
            </tr>
            <tr>
                <td>TURNED</td>
                <td>0x17</td>
            </tr>
            <tr>
                <td>HORRIFIED</td>
                <td>0x18</td>
            </tr>
            <tr>
                <td>INVULNERABLE</td>
                <td>0x19</td>
            </tr>
            <tr>
                <td>INTERRUPTED</td>
                <td>0x1A</td>
            </tr>
            <tr>
                <td>DAZED</td>
                <td>0x1B</td>
            </tr>
            <tr>
                <td>DISCOVERY</td>
                <td>0x1C</td>
            </tr>
            <tr>
                <td>IMMUNE</td>
                <td>0x1D</td>
            </tr>
            <tr>
                <td>SAPPED</td>
                <td>0x1E</td>
            </tr>
            <tr>
                <td>ENRAGED</td>
                <td>0x1F</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Item Flags</summary>
    <table>
        <thead>
            <tr>
                <th>Flag Name</th>
                <th>Flag Id</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>NONE</td>
                <td>0x0</td>
            </tr>
            <tr>
                <td>SOULBOUND</td>
                <td>0x1</td>
            </tr>
            <tr>
                <td>CONJURED</td>
                <td>0x2</td>
            </tr>
            <tr>
                <td>LOOTABLE</td>
                <td>0x4</td>
            </tr>
            <tr>
                <td>WRAP_GIFT</td>
                <td>0x200</td>
            </tr>
            <tr>
                <td>CREATE_ITEM</td>
                <td>0x400</td>
            </tr>
            <tr>
                <td>QUEST</td>
                <td>0x800</td>
            </tr>
            <tr>
                <td>REFUNDABLE</td>
                <td>0x1000</td>
            </tr>
            <tr>
                <td>SIGNABLE</td>
                <td>0x2000</td>
            </tr>
            <tr>
                <td>READABLE</td>
                <td>0x4000</td>
            </tr>
            <tr>
                <td>EVENT_REQ</td>
                <td>0x10000</td>
            </tr>
            <tr>
                <td>PROSPECTABLE</td>
                <td>0x40000</td>
            </tr>
            <tr>
                <td>UNIQUE_EQUIP</td>
                <td>0x80000</td>
            </tr>
            <tr>
                <td>THROWN</td>
                <td>0x400000</td>
            </tr>
            <tr>
                <td>SHAPESHIFT_OK</td>
                <td>0x800000</td>
            </tr>
            <tr>
                <td>ACCOUNT_BOUND</td>
                <td>0x8000000</td>
            </tr>
            <tr>
                <td>MILLABLE</td>
                <td>0x20000000</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>Описание флагов TraceLine</summary>
    <table>
        <thead>
            <tr>
                <th>Название</th>
                <th>ID</th>
                <th>Описание</th>
            </tr>
        </thead>
        <tbody>
            <tr>
            <td>HitTestNothing</td>
            <td>0x0</td>
            <td>No hit test</td>
            </tr>
            <tr>
                <td>HitTestBoundingModels</td>
                <td>0x1</td>
                <td>Models' bounding, ie. where you can't walk on a model. (Trees, smaller structures etc.)</td>
            </tr>
            <tr>
                <td>HitTestWMO</td>
                <td>0x10</td>
                <td>Structures like big buildings, Orgrimmar etc.</td>
            </tr>
            <tr>
                <td>HitTestUnknown</td>
                <td>0x40</td>
                <td>Used in ClickTerrain.</td>
            </tr>
            <tr>
                <td>HitTestGround</td>
                <td>0x100</td>
                <td>The terrain.</td>
            </tr>
            <tr>
                <td>HitTestLiquid</td>
                <td>0x10000</td>
                <td>Tested on water - should work on lava and other liquid as well.</td>
            </tr>
            <tr>
                <td>HitTestUnknown2</td>
                <td>0x20000</td>
                <td>This flag works for liquid as well, but it also works for something else that I don't know (this hit while the liquid flag didn't hit) - called constantly by WoW.</td>
            </tr>
            <tr>
                <td>HitTestMovableObjects</td>
                <td>0x100000</td>
                <td>Hits on movable objects - tested on UC elevator doors.</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>GameObject Types</summary>
    <table>
        <thead>
            <tr>
                <th>Type Name</th>
                <th>Type_ID</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>DOOR</td>
                <td>0</td>
            </tr>
            <tr>
                <td>BUTTON</td>
                <td>1</td>
            </tr>
            <tr>
                <td>QUESTGIVER</td>
                <td>2</td>
            </tr>
            <tr>
                <td>CHEST</td>
                <td>3</td>
            </tr>
            <tr>
                <td>BINDER</td>
                <td>4</td>
            </tr>
            <tr>
                <td>GENERIC</td>
                <td>5</td>
            </tr>
            <tr>
                <td>TRAP</td>
                <td>6</td>
            </tr>
            <tr>
                <td>CHAIR</td>
                <td>7</td>
            </tr>
            <tr>
                <td>SPELL_FOCUS</td>
                <td>8</td>
            </tr>
            <tr>
                <td>TEXT</td>
                <td>9</td>
            </tr>
            <tr>
                <td>GOOBER</td>
                <td>10</td>
            </tr>
            <tr>
                <td>TRANSPORT</td>
                <td>11</td>
            </tr>
            <tr>
                <td>AREADAMAGE</td>
                <td>12</td>
            </tr>
            <tr>
                <td>CAMERA</td>
                <td>13</td>
            </tr>
            <tr>
                <td>MAP_OBJECT</td>
                <td>14</td>
            </tr>
            <tr>
                <td>MO_TRANSPORT</td>
                <td>15</td>
            </tr>
            <tr>
                <td>DUEL_ARBITER</td>
                <td>16</td>
            </tr>
            <tr>
                <td>FISHINGNODE</td>
                <td>17</td>
            </tr>
            <tr>
                <td>SUMMONING_RITUAL</td>
                <td>18</td>
            </tr>
            <tr>
                <td>MAILBOX</td>
                <td>19</td>
            </tr>
            <tr>
                <td>DO_NOT_USE</td>
                <td>20</td>
            </tr>
            <tr>
                <td>GUARDPOST</td>
                <td>21</td>
            </tr>
            <tr>
                <td>SPELLCASTER</td>
                <td>22</td>
            </tr>
            <tr>
                <td>MEETINGSTONE</td>
                <td>23</td>
            </tr>
            <tr>
                <td>FLAGSTAND</td>
                <td>24</td>
            </tr>
            <tr>
                <td>FISHINGHOLE</td>
                <td>25</td>
            </tr>
            <tr>
                <td>FLAGDROP</td>
                <td>26</td>
            </tr>
            <tr>
                <td>MINI_GAME</td>
                <td>27</td>
            </tr>
            <tr>
                <td>DO_NOT_USE_2</td>
                <td>28</td>
            </tr>
            <tr>
                <td>CAPTURE_POINT</td>
                <td>29</td>
            </tr>
            <tr>
                <td>AURA_GENERATOR</td>
                <td>30</td>
            </tr>
            <tr>
                <td>DUNGEON_DIFFICULTY</td>
                <td>31</td>
            </tr>
            <tr>
                <td>BARBER_CHAIR</td>
                <td>32</td>
            </tr>
            <tr>
                <td>DESTRUCTIBLE_BUILDING</td>
                <td>33</td>
            </tr>
            <tr>
                <td>GUILD_BANK</td>
                <td>34</td>
            </tr>
            <tr>
                <td>TRAPDOOR</td>
                <td>35</td>
            </tr>
        </tbody>
    </table>
</details>

