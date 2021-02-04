using NUnit.Framework;
using OpenQA.Selenium;
using System.Threading;

namespace MetroParametersAvitoTests
{
    /*Тест для проверки совпадений колличества станция на сайте
     * с актуальным колличеством.
     * 
     * Его можно подключить к библиотекам и проверять каждую станцию.
     * так же можно немного переработать и тестировать верно ли отображается 
     * выбранное колличетво станций.
     * Я бы сделал его лучше и соверненнее, если бы мог уточнить вопросы у опытных
     * наставников. Спасибо :)
     */
     
     

    
    public class Tests
    {
        //Тут выполняется присвоение 
       
        private IWebDriver driver;
        //Обявлениен переменных, которые содержат XPath
        private readonly By _functionButton = By.XPath("//span[@class = '_3Gk07 _2Vm_X']");

        private readonly By _metroButton = By.XPath("//span[@class = 'css-1bsvtvg']");

        private readonly By _metroLineButton = By.XPath("//button[@tabindex='-1']");

        private readonly By _metroThisLine = By.XPath("//span[text() = 'Арбатско-Покровская']");

        private readonly By _allStations = By.XPath("//span[@class = '_2Jon7'][contains(text(), 'Все станции линии')]");

        private readonly By _abcStationsButton = By.XPath("//button[@data-marker='metro-select-dialog/tabs/button(stations)']");
        
        private readonly By _applyMetroStationsButton = By.XPath("//button[@data-marker='metro-select-dialog/apply'] ");

        //Тут можно подключить библиотеки для взаимодействия с другими линиями метро
        //Я беру фиксированное значение колличества станций на "Арбатско-Покровской" ветке

        private const string _quantityStations = "22";

    [SetUp]
        public void Setup()
        {
            //Присвоение экземпляру ChromeDriver для взаимодействия с браузером
            driver = new OpenQA.Selenium.Chrome.ChromeDriver();
            driver.Navigate().GoToUrl("https://m.avito.ru/moskva/kommercheskaya_nedvizhimost?cd=1&searchForm=true");
            driver.Manage().Window.Maximize(); 
        }
       
        [Test]
        public void Test1()
        {
           

            var function = driver.FindElement(_functionButton);
            function.Click();

            var metroButton = driver.FindElement(_metroButton);
            metroButton.Click();

            var metroLineButton = driver.FindElement(_metroLineButton);
            metroLineButton.Click();

            //Ожидавние чтобы браузер успевал за тестом
            Thread.Sleep(400);
            var metroThisLine = driver.FindElement(_metroThisLine);
            metroThisLine.Click();

            var allStations = driver.FindElement(_allStations);
            allStations.Click();

            var abcMetroStations = driver.FindElement(_abcStationsButton);
            abcMetroStations.Click();

            var applyMetroStationsButton = driver.FindElement(_applyMetroStationsButton);
            applyMetroStationsButton.Click();

            Thread.Sleep(600);
            
            var actualQuantity = driver.FindElement(_applyMetroStationsButton).Text;

            Thread.Sleep(200);
            //Сравнение ожидаемых значений с актуальными
            Assert.AreEqual(_quantityStations, actualQuantity, "the number of stations does not match");


        }

        [TearDown]
        public void TearDown()
        {
            driver.Quit();

        }
    }
}
