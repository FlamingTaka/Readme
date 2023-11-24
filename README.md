using System;
using System.Collections;
using System.Collections.Generic;
using UnityEditor.SearchService;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class PlayerController : MonoBehaviour
{
    Rigidbody2D rb;

    bool isGameActive = true;

    public float jumpForce;

    public GameObject loseScreenUI ;

    public int hiScore;
    public Text scoreUI, hiScoreUI;
    string HISCORE = "HISCORE";
    private int score = 0;

    private void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
    }
    // Start is called before the first frame update
    void Start()
    {
       hiScore = PlayerPrefs.GetInt(HISCORE);
        UpdateHiScoreUI();
    }

    private void UpdateHiScoreUI()
    {
        throw new NotImplementedException();
    }

    // Update is called once per frame
    void Update()
    {
        if (isGameActive)
        {
            PlayerJump();
        }
    }     
    
    void PlayerJump()
    {
        if (Input.GetMouseButtonUp(0))
        {
            rb.velocity = Vector2.up * jumpForce;     
        }
    }

  void PlayerLose()
    {
        if (score > hiScore)
        {
            hiScore = score;
            PlayerPrefs.SetInt(HISCORE, hiScore);
            UpdateHiScoreUI();
        }

        hiScoreUI.text = "HiScore: " + hiScore.ToString();

        loseScreenUI.SetActive(true);
        Time.timeScale = 0;
    }

    public void RestartGame()
    {
        Time.timeScale = 1;
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }

    void AddScore()
    {
        score++;
        scoreUI.text = "score: " + score.ToString();
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.collider.CompareTag("obstacle"))
        {
            PlayerLose();
            //mati
        }
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.CompareTag("score"))
        {
            AddScore();
        }
    }

}
