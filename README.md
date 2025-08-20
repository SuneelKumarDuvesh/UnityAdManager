Unity Ad Monetization System
A centralized, modular, and scalable ad management system for Unity. This package provides a clean API to easily implement rewarded, interstitial, and banner ads using the Unity Ads SDK, designed to be robust for production and flexible for rapid development.

Features
✨ Centralized Management: A single AdsManager singleton acts as the main entry point for all ad calls, keeping your code clean and organized.

🧩 Modular Architecture: Each ad type (Rewarded, Interstitial, Banner) is handled by its own specialized script, making the system easy to maintain and extend.

🔌 Decoupled Design: UI and gameplay scripts only need to know about the AdsManager, not the underlying ad SDK. This makes it incredibly easy to swap ad providers (e.g., from Unity Ads to AdMob) in the future without refactoring your entire game.

🏆 Flexible Reward Handling: The API supports both specific, context-based rewards (e.g., "Grant 100 Coins") and global events for system-wide notifications (e.g., for analytics).

⚙️ Powerful API: Uses C# method overloading to provide a simple API for both quick testing (ShowRewarded()) and specific, production-ready implementations (ShowRewarded(Action onRewardGranted)).

🔁 Automatic Retry Logic: Built-in exponential backoff for failed ad loads ensures a resilient and reliable ad experience.

Requirements
Unity 2021.3 LTS or newer.

Unity Advertisement Legacy Package: This system is built on the Unity Ads SDK. You must install it from the Unity Package Manager.

Go to Window > Package Manager.

Select Unity Registry.

Search for Advertisement Legacy and click Install.

Installation
Go to the Releases page of this GitHub repository.

Download the latest AdSystem_vX.X.X.unitypackage file.

Open your Unity project.

Import the package by going to Assets > Import Package > Custom Package... and selecting the downloaded file.

Setup in Unity
To get the system running in your scene, follow these steps:

Create Manager GameObject: Create a new, empty GameObject in your main scene and name it _AdsManager.

Add All Scripts: Select the _AdsManager GameObject. In the Inspector, use the Add Component button to add all four of the main ad scripts:

AdsManager

RewardedAdScript

InterstitialAdScript

BannerAdScript

Assign References: The AdsManager component has three empty fields. Click and drag the name of each corresponding script component from the Inspector into its correct slot.

Your final setup should look like this:

The system is now ready to use!

How to Use


The AdsManager is a singleton, so you can access it from any script using AdsManager.Instance.

Example 1: Showing an Interstitial Ad
Create a simple handler script and attach it to your button.
// In a script like "LevelEndUI.cs"


using UnityEngine;
public class LevelEndUI : MonoBehaviour
{

    // Link this method to a button's OnClick() event in the Inspector.
    public void OnNextLevelButtonClicked()
    
    {
    
        // Simply ask the AdsManager to show an interstitial ad.
        AdsManager.Instance.ShowInterstitial();
        
        // Your code to load the next level...
    }
}



Example 2: Showing a Rewarded Ad (Recommended Pattern)
Create a reusable handler script for your rewarded ad buttons.
In a script like "AdButtonHandler.cs"



using UnityEngine;
public class AdButtonHandler : MonoBehaviour
{

    // Link this method to your "Get Coins" button's OnClick() event.
    
    public void OnWatchAdForCoins()
    {
        // Tell the AdsManager to show an ad and what the reward is.
        AdsManager.Instance.ShowRewarded(GrantCoinsReward);
    }

    private void GrantCoinsReward()
    {
        Debug.Log("REWARD GRANTED: 100 Coins!");
        // Your game logic to give the player 100 coins.
    }

    // You can add more methods for different rewards.
    public void OnWatchAdForGems()
    {
        AdsManager.Instance.ShowRewarded(GrantGemsReward);
    }

    private void GrantGemsReward()
    {
        Debug.Log("REWARD GRANTED: 10 Gems!");
        // Your game logic to give the player 10 gems.
    }
}

API Overview
These are the main public methods available in the AdsManager:

Method

Description

ShowRewarded(Action onRewardGranted)

(Recommended) Shows a rewarded ad and executes the specific onRewardGranted function on completion.

ShowRewarded()

Shows a rewarded ad without a specific callback. Fires the global OnRewardedAdCompleted event.

ShowInterstitial()

Shows a standard interstitial ad.

ShowBanner(BannerPosition position)

Shows a banner ad at a specific, customized screen position.

ShowBanner()

Shows a banner ad at the default position set in the BannerAdScript Inspector.

HideBanner()

Hides the currently visible banner ad.

License
This project is licensed under the MIT License. See the LICENSE file for details.
