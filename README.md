# Şehir Turizmi uygulaması

istenilen şehir için turizm uygulaması

## Açıklama

Ödevde unity adlı uygulamayı kullandım database olarak ise firebase'i kullandım




## Scriptler

```c#
using System.Collections;
using System.Collections.Generic;
using Firebase;
using Firebase.Auth;
using Firebase.Extensions;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class FirebaseUyeIslemleri : MonoBehaviour {

    [Header ("Giris Yap UI")]
    [SerializeField] InputField girisEmail;
    [SerializeField] InputField girisSifre;

    [Header ("Uye Ol UI")]
    [SerializeField] InputField uyeOlEmail;
    [SerializeField] InputField uyeOlSifre;
    [SerializeField] InputField uyeOlSifreKontrol;

    FirebaseAuth auth;
    
    void Awake () {
        auth = FirebaseAuth.DefaultInstance;
    }
    void Start () {
        auth.SignOut();

        auth.StateChanged += AuthStateChange;
      
    }

    void AuthStateChange(object sender, System.EventArgs eventArgs)
    {
        if (auth.CurrentUser != null)
        {
            SceneManager.LoadScene("GameScene");
        }
    }

    public void UyeOl () {
        if (UyeOlVeriKontrol ()) {
            
            auth.CreateUserWithEmailAndPasswordAsync (uyeOlEmail.text, uyeOlSifre.text).ContinueWithOnMainThread (task => {
                if (task.IsCanceled) {
                    Debug.Log ("İptal Edildi");
                    return;
                }
                if (task.IsFaulted) {
                    Debug.LogError ("CreateUserWithEmailAndPasswordAsync encountered an error: " + task.Exception);
                    return;
                }

                Firebase.Auth.FirebaseUser newUser = task.Result;
                Debug.LogFormat ("Firebase user created successfully: {0} ({1})",
                    newUser.DisplayName, newUser.UserId);
            });

        } else {
            Debug.LogWarning ("Alanlar hatalı");
        }
    }

    bool UyeOlVeriKontrol () {

        if (uyeOlEmail.text == null || uyeOlEmail.text == "") {
            return false;
        }
        if (uyeOlSifre.text == null || uyeOlSifre.text == "" || uyeOlSifreKontrol.text == null || uyeOlSifre.text == "") {
            return false;
        }

        if (uyeOlSifre.text != uyeOlSifreKontrol.text) {
            return false;
        }

        return true;
    }

    public void UyeGirisi () {

        if (GirisVeriKontrol ()) {
            auth.SignInWithEmailAndPasswordAsync (girisEmail.text, girisSifre.text).ContinueWith (task => {
                if (task.IsCanceled) {
                    Debug.LogError ("SignInWithEmailAndPasswordAsync was canceled.");
                    return;
                }
                if (task.IsFaulted) {
                    Debug.LogError ("SignInWithEmailAndPasswordAsync encountered an error: " + task.Exception);
                    return;
                }

                Firebase.Auth.FirebaseUser newUser = task.Result;
                Debug.LogFormat ("User signed in successfully: {0} ({1})",
                    newUser.DisplayName, newUser.UserId);
                if (auth.CurrentUser != null)
                {

                    SceneManager.LoadScene("GameScene");
                    Debug.Log(auth.CurrentUser);

                }
            });
        }
    }
    bool GirisVeriKontrol () {

        if (girisEmail.text == null || girisEmail.text == "") {
            return false;
        }
        if (girisSifre.text == null || girisSifre.text == "") {
            return false;
        }
        return true;
    }
}
```
```c#
using System;
using System.Collections;
using System.Collections.Generic;
using Firebase;
using Firebase.Auth;
using Firebase.Database;
using Firebase.Extensions;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
public class scene : MonoBehaviour
{
    [SerializeField] GameObject parklarPanel;
    [SerializeField] GameObject kutuphanelerPanel;
    [SerializeField] GameObject tarihiyerlerPanel;
    [SerializeField] GameObject otellerPanel;
    [SerializeField] GameObject marketlerPanel;
    [SerializeField] GameObject ibadetyerleriPanel;
    [SerializeField] GameObject otoparklarPanel;
    FirebaseAuth auth;
    DatabaseReference reference;
    void Awake()
    {
        auth = FirebaseAuth.DefaultInstance;
    }
    void Start()
    {
        if (auth.CurrentUser == null)
        {
            SceneManager.LoadScene("StartScene");
        }
        else
        {

            reference = FirebaseDatabase.DefaultInstance.RootReference;
        }
    }

    public void parklar()
    {
        parklarPanel.SetActive(true);

    }
    public void kutuphaneler()
    {
        kutuphanelerPanel.SetActive(true);

    }
    public void tarihiyerler()
    {
        tarihiyerlerPanel.SetActive(true);

    }
    public void oteller()
    {
        otellerPanel.SetActive(true);

    }
    public void marketler()
    {
        marketlerPanel.SetActive(true);

    }
    public void ibadetyerleri()
    {
        ibadetyerleriPanel.SetActive(true);

    }
    public void otoparklar()
    {
        otoparklarPanel.SetActive(true);

    }
    public void kapatici()
    {
        parklarPanel.SetActive(false);
        kutuphanelerPanel.SetActive(false);
        tarihiyerlerPanel.SetActive(false);
        otellerPanel.SetActive(false);
        marketlerPanel.SetActive(false);
        ibadetyerleriPanel.SetActive(false);
        otoparklarPanel.SetActive(false);

    }


}

```
```c#
using System;
using System.Collections;
using System.Collections.Generic;
using Firebase;
using Firebase.Auth;
using Firebase.Database;
using Firebase.Extensions;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
using TMPro;
public class parklarscript : MonoBehaviour
{
    [SerializeField] TextMeshProUGUI yazi;
    [SerializeField] TextMeshProUGUI yorum;
    [SerializeField] TextMeshProUGUI yazik;
    [SerializeField] TextMeshProUGUI yorumk;
    [SerializeField] TextMeshProUGUI yazit;
    [SerializeField] TextMeshProUGUI yorumt;
    [SerializeField] TextMeshProUGUI yazio;
    [SerializeField] TextMeshProUGUI yorumo;
    [SerializeField] TextMeshProUGUI yazim;
    [SerializeField] TextMeshProUGUI yorumm;
    [SerializeField] TextMeshProUGUI yazii;
    [SerializeField] TextMeshProUGUI yorumi;
    [SerializeField] TextMeshProUGUI yaziot;
    [SerializeField] TextMeshProUGUI yorumot;
    [SerializeField] GameObject parklarPanel;
    [SerializeField] GameObject kutuphanelerPanel;
    [SerializeField] GameObject tarihiyerlerPanel;
    [SerializeField] GameObject otellerPanel;
    [SerializeField] GameObject marketlerPanel;
    [SerializeField] GameObject ibadetyerleriPanel;
    [SerializeField] GameObject otoparklarPanel;
    FirebaseAuth auth;
    DatabaseReference reference;
    void Start()
    {
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Return))
        {
            if(parklarPanel.active== true)
            {
                yorum.text += "\n\n";
                yorum.text += yazi.text;
            }
            if (kutuphanelerPanel.active == true)
            {
                yorumk.text += "\n\n";
                yorumk.text += yazik.text;
            }
            if (tarihiyerlerPanel.active == true)
            {
                yorumt.text += "\n\n";
                yorumt.text += yazit.text;
            }
            if (otellerPanel.active == true)
            {
                yorumo.text += "\n\n";
                yorumo.text += yazio.text;
            }
            if (marketlerPanel.active == true)
            {
                yorumm.text += "\n\n";
                yorumm.text += yazim.text;
            }
            if (ibadetyerleriPanel.active == true)
            {
                yorumi.text += "\n\n";
                yorumi.text += yazii.text;
            }
            if (otoparklarPanel.active == true)
            {
                yorumot.text += "\n\n";
                yorumot.text += yaziot.text;
            }          
        }
    }
}

```
<p><img align = "center" src = "https://github.com/OguzhanUgur1/sehirRehberi/blob/main/My%20project%201%202022-06-08_22-19-37.gif" width = "1080" height = "600" /></p>
