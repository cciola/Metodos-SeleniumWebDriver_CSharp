# Executar mouseover (hover) com JavascriptExecutor

Após criar um Unit Test Project e importar as bibliotecas abaixo:
```
NUnit
NUnit 3 - NUnit Project Loader Extension
NUnit 3 - NUnit V2 Framework Driver Extension
NUnit 3 - NUnit V2 Result Writer Extension
NUnit 3 - Team City Event Listener Extension
NUnit 3 - Visual Studio Project Loader Extension
NUnit Console Runner Version 3 (No Extensions)
NUnit Console Runner Version 3 With Extensions
NUnit Console Version 3
NUnit Test Adapter for VS2012, VS2013 ans VS2015
Selenium WebDriver
Selenium WebDriver Support Classes
Selenium.WebDriver.ChromeDriver //caso deseje executar testes no navegador Internet Explorer
Selenium.WebDriver.IEDriver //caso deseje executar testes no navegador Internet Explorer
Selenium.WebDriver.Firefox //caso deseje executar testes no navegador Internet Explorer
Selenium.Support
```
Na classe pública do projeto, localizada logo abaixo do [TextFixture], informe a seguinte variável:
```
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        IJavaScriptExecutor js; // Javascript Executor
```
Declare antes do código do teste, no [Test]:
```
js = (IJavaScriptExecutor)driver; //Permite executar Javascript
```
No script de teste, utilizamos os seguintes comandos:
```
IWebElement menu = driver.FindElement(By.LinkText("MENU PRINCIPAL")); //link do menu principal
js.ExecuteScript("arguments[0].onmouseover()", menu);
Thread.Sleep(3000);
```
# Veja o método funcionando

Copie e cole o código abaixo, dê um Build e execute o teste.
```
using System;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading;
using System.Drawing.Imaging;
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Firefox;
using OpenQA.Selenium.Support.UI;
using OpenQA.Selenium.Chrome;
using System.IO;
using System.Collections;

namespace SeleniumTests
{
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        private string baseURL;
        IJavaScriptExecutor js; // Javascript Executor

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            baseURL = "http://www.seusite.com.br"; //informe o site desejado
        }

        [TearDown]
        public void TeardownTest()
        {
            try
            {
                driver.Quit();
            }
            catch (Exception)
            {
                // Ignore errors if unable to close the browser
            }
        }

        [Test]

        public void NomeDoTeste()
        {

            js = (IJavaScriptExecutor)driver; //Permite executar Javascript

            driver.Navigate().GoToUrl(baseURL);
            Thread.Sleep(1000);
            IWebElement menu = driver.FindElement(By.LinkText("NomeDoMenu")); //informe o nome do menu principal
            js.ExecuteScript("arguments[0].onmouseover()", menu);
            Thread.Sleep(3000);
            driver.Quit();
        }
    }
}
```
O código acessa a página base desejada, posiciona o mouse sobre o menu dropdown desejado e exibe seus submenus.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
