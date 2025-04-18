using System;
using System.Formats.Asn1;
using System.Linq;

namespace SpartaDungeon
{
    class Item
    {
        public string Name;
        public string Description;
        public int Power;
        public bool IsEquipped; // <-- 장착 여부
        public ItemType Type;

        public enum ItemType
        {
            Attack,
            Defense
        }


        public Item(string itemname, string description, int power, ItemType type)
        {
            Name = itemname;
            Description = description;
            Power = power;
            Type = type;
            IsEquipped = false;
        }
        public string Display()
        {
            return (IsEquipped ? "[E] " : "") + Name + " | " + Description;
        }

    }

    internal class Program
    {
        static List<Item> inventory = new List<Item>()
        {
        new Item("무쇠갑옷", "방어력 +5 | 무쇠로 만들어져 튼튼한 갑옷입니다.", 5, Item.ItemType.Defense),
        new Item("스파르타의 창", "공격력 +7 | 스파르타의 전사들이 사용했다는 전설의 창입니다.", 7, Item.ItemType.Attack),
        new Item("낡은 검", "공격력 +2 | 쉽게 볼 수 있는 낡은 검 입니다.", 2, Item.ItemType.Attack)
        };       

        // static List<ShopItem> shopItems = new List<ShopItem>()
        // {
        //     new ShopItem("수련자 갑옷", "방어력 +5 | 수련에 도움을 주는 갑옷입니다.", 1000),
        //     new ShopItem("무쇠갑옷", "방어력 +9 | 무쇠로 만들어져 튼튼한 갑옷입니다.", 2000),
        //     new ShopItem("스파르타의 갑옷", "방어력 +15 | 스파르타의 전사들이 사용했다는 전설의 갑옷입니다.", 3500),
        //     new ShopItem("낡은 검", "공격력 +2 | 쉽게 볼 수 있는 낡은 검 입니다.", 600),
        //     new ShopItem("청동 도끼", "공격력 +5 | 어디선가 사용됐던거 같은 도끼입니다.", 1500),
        //     new ShopItem("스파르타의 창", "공격력 +7 | 스파르타의 전사들이 사용했다는 전설의 창입니다.", 3000)
        // };
    

        static void Main(string[] args)
        {
            while (true) // 시작 화면 반복
            {
                Console.Clear();

                Console.WriteLine("스파르타 던전에 오신 여러분 환영합니다!");
                Console.WriteLine("이름을 입력해주세요.\n");
                string name = Console.ReadLine();

                // 저장 여부 확인
                bool isSaved = false;

                while (!isSaved)
                {
                    Console.Clear();
                    Console.WriteLine("입력하신 이름은 {0}입니다.", name);
                    Console.WriteLine("\n1. 저장 \n2. 취소");
                    Console.WriteLine("\n원하시는 번호를 입력해주세요.\n");

                    string saveInput = Console.ReadLine();

                    if (saveInput == "1")
                    {
                        Console.WriteLine("저장되었습니다.");
                        isSaved = true; // 저장 성공
                    }
                    else if (saveInput == "2")
                    {
                        Console.WriteLine("이름 입력을 취소하셨습니다. 다시 입력해주세요.");
                        Console.ReadKey(); // 잠깐 멈추고 다시 이름 입력으로
                        break;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다. 다시 시도해주세요.");
                        Console.ReadKey(); // 틀리면 다시 입력
                    }
                }

                // 저장 안 됐으면 이름 입력으로 돌아감
                if (!isSaved)
                    continue;

                // 직업 선택
                bool jobSelected = false;

                while (!jobSelected)
                {
                    Console.Clear();

                    Console.WriteLine("반갑습니다 {0} 님!", name);
                    Console.WriteLine("\n직업을 선택해주세요.");
                    Console.WriteLine("\n1. 전사 \n2. 도적\n");

                    string job = Console.ReadLine();

                    if (job == "1")
                    {
                        Console.WriteLine("\n전사를 선택하셨습니다!");
                        Console.ReadKey();
                        jobSelected = true;
                    }
                    else if (job == "2")
                    {
                        Console.WriteLine("\n도적을 선택하셨습니다!");
                        Console.ReadKey();
                        jobSelected = true;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다. 다시 선택해주세요.");
                        Console.ReadKey(); // 틀리면 다시 입력
                        
                    }
                }

                
                while (true)
                {
                    
                    Console.Clear();
                    Console.WriteLine("\n스파르타 마을에 오신 여러분 환영합니다!");
                    Console.WriteLine("이곳에서 던전에 들어가기 전 활동을 할 수 있습니다.");
                    Console.WriteLine("\n1. 상태 보기 \n2. 인벤토리 \n3. 상점");                   
                    Console.WriteLine("\n원하시는 행동을 입력해주세요.");
                    Console.Write(">> ");
                    string town = Console.ReadLine();

                    if (town == "1")
                    {
                        int baseAtk = 10;
                        int baseDef = 5;

                        Console.Clear();
                        Console.WriteLine("상태 보기");
                        Console.WriteLine("캐릭터의 정보가 표시됩니다.");

                        Console.WriteLine("\nLv. 01");
                        string job = jobSelected ? "전사" : "도적";
                        // 직업에 따라 다른 출력
                        Console.WriteLine("{0} ( {1} )", name, job == "1" ? "전사" : "도적");

                        // 장착된 아이템의 추가 능력치 계산
                        int bonusAtk = inventory.Where(i => i.IsEquipped && i.Type == Item.ItemType.Attack).Sum(i => i.Power);
                        int bonusDef = inventory.Where(i => i.IsEquipped && i.Type == Item.ItemType.Defense).Sum(i => i.Power);

                        // 상태창 출력
                        Console.WriteLine("공격력 : {0} (+{1})", baseAtk, bonusAtk);
                        Console.WriteLine("방어력 : {0} (+{1})", baseDef, bonusDef);

                        Console.WriteLine("체 력 : 100");
                        Console.WriteLine("Gold : 1500 G");

                        Console.WriteLine("\n0. 나가기");
                        Console.WriteLine("\n원하시는 행동을 입력해주세요.");
                        Console.Write(">> ");
                        
                        string statInput = Console.ReadLine();

                        if (statInput == "0")
                        {
                            continue; // 나가기
                        }
                        while (statInput != "0")
                        {
                            Console.WriteLine("잘못된 입력입니다. 다시 시도해주세요.");
                            statInput = Console.ReadLine();
                        }
                    }
                    else if (town == "2")
                    {
                        while (true)
                        {
                            // 인벤토리
                            Console.Clear();
                            Console.WriteLine("인벤토리");
                            Console.WriteLine("보유 중인 아이템을 관리할 수 있습니다.");

                            Console.WriteLine("\n[아이템 목록]");
                            for (int i = 0; i < inventory.Count; i++)
                            {
                                Console.WriteLine($"- {inventory[i].Display()}");
                            }

                            Console.WriteLine("\n1. 장착 관리");
                            Console.WriteLine("0. 나가기");

                            Console.WriteLine("\n원하시는 행동을 입력해주세요.");
                            Console.Write(">> ");

                            string inventoryInput = Console.ReadLine();


                            if (inventoryInput == "0")
                            {
                                break;
                            }
                            else if (inventoryInput == "1")
                            {
                                bool isExit = true;
                                while (isExit)
                                {
                                    // 장착 관리
                                    Console.Clear();
                                    Console.WriteLine("인벤토리 - 장착 관리");
                                    Console.WriteLine("보유 중인 아이템을 관리 할 수 있습니다.");

                                    Console.WriteLine("\n[아이템 목록]");

                                    for (int i = 0; i < inventory.Count; i++)
                                    {
                                        Console.WriteLine($"{i + 1} {inventory[i].Display()}");
                                    }

                                    Console.WriteLine("\n0. 나가기");
                                    Console.WriteLine("\n원하시는 행동을 입력해주세요.");
                                    Console.Write(">> ");

                                    string equipInput = Console.ReadLine();
                                    int equipIndex;

                                    if (int.TryParse(equipInput, out equipIndex) && equipIndex > 0 && equipIndex <= inventory.Count)
                                    {
                                        Item selectedItem = inventory[equipIndex - 1];

                                        if (selectedItem.IsEquipped)
                                        {
                                            // 이미 장착되어 있으면 해제
                                            selectedItem.IsEquipped = false;
                                            
                                        }
                                        else
                                        {
                                            // 장착되어 있지 않으면 장착
                                            selectedItem.IsEquipped = true;
                                            
                                        }
                                    }
                                    else if (equipInput == "0")
                                    {
                                        isExit = false; // 장착 관리 종료 → 인벤토리 메뉴로 돌아감
                                        break;
                                        
                                    }
                                    else
                                    {
                                        Console.WriteLine("잘못된 입력입니다. 다시 시도해주세요.");
                                        equipInput = Console.ReadLine();
                                    }
                                
                                }
                            }
                            else
                            {
                                Console.WriteLine("잘못된 입력입니다. 다시 입력해주세요.");
                                Console.ReadKey();
                            }

                        }
                        
                    }
                    else if (town == "3")
                    {
                        bool isInShop = true;

                        while (isInShop)
                        {
                            Console.Clear();
                            Console.WriteLine("상점 \n필요한 아이템을 얻을 수 있는 상점입니다.");

                            Console.WriteLine("\n[보유 골드]");
                            Console.WriteLine("800 G");

                            Console.WriteLine("\n[아이템 목록]");
                            Console.WriteLine("- 수련자 갑옷    | 방어력 +5  | 수련에 도움을 주는 갑옷입니다.                 |  1000 G");
                            Console.WriteLine("- 무쇠갑옷      | 방어력 +9  | 무쇠로 만들어져 튼튼한 갑옷입니다.               |  구매완료");
                            Console.WriteLine("- 스파르타의 갑옷 | 방어력 +15 | 스파르타의 전사들이 사용했다는 전설의 갑옷입니다.    |  3500 G");
                            Console.WriteLine("- 낡은 검      | 공격력 +2  | 쉽게 볼 수 있는 낡은 검 입니다.                |  600 G");
                            Console.WriteLine("- 청동 도끼     | 공격력 +5  | 어디선가 사용됐던거 같은 도끼입니다.            |  1500 G");
                            Console.WriteLine("- 스파르타의 창  | 공격력 +7  | 스파르타의 전사들이 사용했다는 전설의 창입니다.     |  구매완료");

                            Console.WriteLine("\n1. 아이템 구매");
                            Console.WriteLine("0. 나가기");

                            Console.Write("\n>> ");
                            string shopInput = Console.ReadLine();

                            if (shopInput == "0")
                            {
                                isInShop = false;
                            }
                            else if (shopInput == "1")
                            {
                                bool isBuying = true;
                                while (isBuying)
                                {
                                    Console.Clear();
                                    Console.WriteLine("아이템 구매");
                                    Console.WriteLine("구매할 아이템의 번호를 입력해주세요.");

                                    Console.WriteLine("\n[보유 골드]");
                                    Console.WriteLine("800 G");

                                    Console.WriteLine("\n[아이템 목록]");
                                    Console.WriteLine("1. 수련자 갑옷    | 방어력 +5  | 수련에 도움을 주는 갑옷입니다.                 |  1000 G");
                                    Console.WriteLine("2. 무쇠갑옷      | 방어력 +9  | 무쇠로 만들어져 튼튼한 갑옷입니다.               |  구매완료");
                                    Console.WriteLine("3. 스파르타의 갑옷 | 방어력 +15 | 스파르타의 전사들이 사용했다는 전설의 갑옷입니다.    |  3500 G");
                                    Console.WriteLine("4. 낡은 검      | 공격력 +2  | 쉽게 볼 수 있는 낡은 검 입니다.                |  600 G");
                                    Console.WriteLine("5. 청동 도끼     | 공격력 +5  | 어디선가 사용됐던거 같은 도끼입니다.            |  1500 G");
                                    Console.WriteLine("6. 스파르타의 창  | 공격력 +7  | 스파르타의 전사들이 사용했다는 전설의 창입니다.     |  구매완료");

                                    Console.WriteLine("\n0. 나가기");
                                    Console.Write("\n>> ");

                                    string itemInput = Console.ReadLine();
                                    int itemNumber;

                                    if (int.TryParse(itemInput, out itemNumber))
                                    {
                                        switch (itemNumber)
                                        {
                                            case 0:
                                                isBuying = false; // 구매 루프 빠져나감 → 상점 메뉴로 돌아감
                                                break;
                                            case 1:
                                                Console.WriteLine("Gold 가 부족합니다.");
                                                Console.ReadKey();
                                                isBuying = true; // 구매 루프 계속 → 아이템 구매 화면 유지
                                                break;
                                            case 2:
                                                Console.WriteLine("이미 구매한 아이템입니다.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                            case 3:
                                                Console.WriteLine("Gold 가 부족합니다.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                            case 4:
                                                Console.WriteLine("구매를 완료했습니다.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                            case 5:
                                                Console.WriteLine("Gold 가 부족합니다.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                            case 6:
                                                Console.WriteLine("이미 구매한 아이템입니다.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                            default:
                                                Console.WriteLine("잘못된 입력입니다. 다시 입력해주세요.");
                                                Console.ReadKey();
                                                isBuying = true;
                                                break;
                                        }
                                    }
                                    else
                                    {
                                        Console.WriteLine("잘못된 입력입니다. 숫자를 입력해주세요.");
                                    }

                                }
                            }
                            else
                            {
                                Console.WriteLine("잘못된 입력입니다. 다시 입력해주세요.");
                                Console.ReadKey();
                            }
                        }
                    }

                }

            }
            
        }
    }
}
