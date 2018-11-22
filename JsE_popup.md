# Exibir popup com JavascriptExecutor

Após criar um Unit Test Project e importar as bibliotecas abaixo:
```csharp
NUnit
NUnit 3 - NUnit Project Loader Extension
NUnit 3 - NUnit V2 Framework Driver Extension
NUnit 3 - NUnit V2 Result Writer Extension
NUnit 3 - Team City Event Listener Extension
NUnit 3 - Visual Studio Project Loader Extension
NUnit Console Runner Version 3 (No Extensions)
NUnit Console Runner Version 3 With Extensions
NUnit Console Version 3
NUnit Test Adapter for VS2012, VS2013 and VS2015
Selenium WebDriver
Selenium WebDriver Support Classes
Selenium.WebDriver.ChromeDriver //caso deseje executar testes no navegador Chrome
Selenium.WebDriver.IEDriver //caso deseje executar testes no navegador Internet Explorer
Selenium.WebDriver.Firefox //caso deseje executar testes no navegador Firefox
Selenium.Support
```
Na classe pública do projeto, localizada logo abaixo do [TextFixture], informe a seguinte variável:
```csharp
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        IJavaScriptExecutor js; // Javascript Executor
```
Declare antes do código do teste, no [Test]:
```csharp
js = (IJavaScriptExecutor)driver; //Permite executar Javascript
```
No script de teste, utilizamos os seguintes comandos:
```csharp      
//exibe o popup com um texto fixo informado
js.ExecuteScript("alert('Script de teste finalizado com sucesso!');"); 
Thread.Sleep(3000);

//exibe o popup com um texto fixo concatenando o valor de uma variável
js.ExecuteScript("alert('CPF gerado: "+GerarCpf()+"');");
Thread.Sleep(3000);

```
# Veja o método funcionando - texto fixo

Copie e cole o código abaixo, dê um Build e execute o teste.
```csharp
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
        IJavaScriptExecutor js; // Javascript Executor
        public string variavel = "Carol";

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
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

            driver.Navigate();
            Thread.Sleep(1000);
            js.ExecuteScript("alert('Script de teste finalizado com sucesso!');");
            Thread.Sleep(3000);
            driver.Quit();
        }
    }
}
```
# Veja o método funcionando - texto + variável

Copie e cole o código abaixo, dê um Build e execute o teste.
```csharp
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
        IJavaScriptExecutor js; // Javascript Executor
        public string variavel = "Carol";

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
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

            driver.Navigate();
            Thread.Sleep(1000);
            js.ExecuteScript("alert('Valor da variavel: "+variavel+"');");
            Thread.Sleep(3000);
            driver.Quit();
        }
    }
}
```
Ambos códigos abrem o navegador e exibem o popup.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
