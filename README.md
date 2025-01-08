using System;
using System.Collections.Generic;

namespace DeliveryApp
{
    public enum MyFoodType
    {
        Italian = 0,
        Asian = 1,
        American = 2,
        Caucasian = 3,
        Georgian = 4,
        Panasian = 5
    }

    // абстрактный класс catering только для хранения информации и работы с коллекцией
    public abstract class Catering
    {
        public int CateringID { get; set; }
        public string Name { get; set; }
        private List<Good> Goods { get; } = new List<Good>(); // коллекция товаров

        // это для добавления заказа
        public string AddGood(Good good)
        {
            Goods.Add(good);
            return $"Товар \"{good.Name}\" добавлен в \"{Name}\".";
        }

        // это для удаления заказа
        public string RemoveGood(Good good)
        {
            if (Goods.Remove(good))
            {
                return $"Товар \"{good.Name}\" удалён из \"{Name}\".";
            }
            else
            {
                return $"Товар \"{good.Name}\" не найден в \"{Name}\".";
            }
        }

        // отображение всех товаров
        public string DisplayGoods()
        {
            if (Goods.Count == 0)
            {
                return $"В \"{Name}\" нет товаров.";
            }

            List<string> goodsList = new List<string> { $"Товары в \"{Name}\":" };
            foreach (var good in Goods)
            {
                goodsList.Add($"- {good.Name} ({good.TypeOfGood})");
            }

            return string.Join(Environment.NewLine, goodsList);
        }
    }

    // класс good модель данных для хранения информации о товаре
    public class Good
    {
        public string Name { get; set; }
        public MyFoodType TypeOfGood { get; set; }
    }

    // класс restaurant 
    public class Restaurant : Catering
    {
    }

    // класс shop
    public class Shop : Catering
    {
    }

    class Program
    {
        static void Main(string[] args)
        {
            // создаем ресторан и магазин
            Restaurant rest = new Restaurant { CateringID = 0, Name = "Итальянский ресторан" };
            Shop shop = new Shop { CateringID = 2, Name = "Магазин продуктов" };

            // создаем товары
            Good pizza = new Good { Name = "Пицца", TypeOfGood = MyFoodType.Italian };
            Good sushi = new Good { Name = "Суши", TypeOfGood = MyFoodType.Asian };
            Good seaweed = new Good { Name = "Водоросли", TypeOfGood = MyFoodType.Panasian };

            // добавляем товары
            rest.AddGood(pizza);
            shop.AddGood(sushi);
            shop.AddGood(seaweed);

            // отображаем товары
            Console.WriteLine(rest.DisplayGoods());
            Console.WriteLine(shop.DisplayGoods());

            // удаляем товар
            shop.RemoveGood(sushi);
            Console.WriteLine(shop.DisplayGoods());

            // пауза, чтобы увидеть результат
            Console.WriteLine("Нажмите любую клавишу для завершения...");
            Console.ReadKey();
        }
    }
}
