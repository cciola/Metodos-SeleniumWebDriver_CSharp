# Criar classe externa

Para que o código-fonte do projeto não fique tão extenso, é possível chamar métodos a partir de classes externas.

No Solution Explorer, clicar com botão direito do mouse no nome do projeto > Add > Class... > Class > informe o nome da classe.

No "namespace" deve ser informado o nome do projeto:
```csharp
namespace NomeDoProjeto
```
A classe criada automaticamente deve ser pública:
```csharp
public class GeraCPF
```
Após criação da classe, no Solution Explorer, clique com botão direito do mouse no nome do projeto > Add > New Folder > e informe o nome da pasta (pode ser o mesmo nome da classe)

Arraste a classe.cs criada para dentro da pasta.

No arquivo .cs do projeto, acrescente o package abaixo:
```csharp
using NomeDoProjeto;
```
No script de teste, instancie a classe:
```csharp
NomeDoArquivoDaClasse n = new "NomeDoArquivoDaClasse();
```

Chamando a classe no script:
```csharp
n.NomeDaClasse();
```

# Veja o método funcionando

Classe GeraCPF criada:
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace AW_cadastroPaciente
{
    public class GeraCPF
    {
        public string GerarCpf()
        {
            int soma = 0, resto = 0;
            int[] multiplicador1 = new int[9] { 10, 9, 8, 7, 6, 5, 4, 3, 2 };
            int[] multiplicador2 = new int[10] { 11, 10, 9, 8, 7, 6, 5, 4, 3, 2 };
            Random rnd = new Random();
            string semente = rnd.Next(100000000, 999999999).ToString();
            for (int i = 0; i < 9; i++)
                soma += int.Parse(semente[i].ToString()) * multiplicador1[i];
            resto = soma % 11;
            if (resto < 2)
                resto = 0;
            else
                resto = 11 - resto;
            semente = semente + resto;
            soma = 0;
            for (int i = 0; i < 10; i++)
                soma += int.Parse(semente[i].ToString()) * multiplicador2[i];
            resto = soma % 11;
            if (resto < 2)
                resto = 0;
            else
                resto = 11 - resto;
            semente = semente + resto;
            return semente;
        }
    }
}
```

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
using NomeDoProjeto; //incluir manualmente

namespace SeleniumTests
{
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        private string baseURL;

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            baseURL = "http://www.google.com.br"; //informe o site desejado
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

            NomeDoArquivoDaClasse n = new "NomeDoArquivoDaClasse();

            driver.Navigate().GoToUrl(baseURL);
            Thread.Sleep(1000);
            driver.FindElement(By.Id("ucProdutos_txtNumeroCartao")).SendKeys(n.GerarCpf());
            Thread.Sleep(2000);
            driver.Quit();
        }
    }
}
```
O código acessa a página base desejada, rola a página para baixo e depois rola a página para cima.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
